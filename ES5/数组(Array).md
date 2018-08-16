##数组`(Array)`
###`创建数组`
> 三种方式:
>  + 使用 new Array( )构造函数;
>  + 直接使用Array( );
>  + 使用数组字面量表示法;

    //第一种方式
    var arr = new Array(3);//new 操作符可以省略
    //括号内写参数是length
    //包含一项
    var name = new Array("hell");//1项
    //第二种方式
    var arr = [];//空数组
    //用逗号分隔
    var arr = ["red","blue","green"];
    var values = [1,2,];//IE8以下会错误识别(有bug),最后一个逗号会正常识别,最后是undefined

**`特点`**
都可通过下标访问和`length`:

    var arr = new Array("1","2","3");
    console.log(arr[2]);//3
    var a = ["1","2","3"];
    console.log(a[2]);//3
    //认为的改变数组长度,最后会赋值为undefined
    a.length = 99;
    console.log(a[98]);//undefined

**`稀疏数组`**

    var arr = [1,,2,3,5];
    console.log(arr.length);//5

**`一个小技巧`**

    <script>
    //判断条件为     !-[1,]
       if( [1,2,].length === 3 ){
            alert("我是IE5678");
        }else{
            alert("我不是IE678");
        }
    </script>

### 检测数组`Array.IsArray`

**`两种方法`**

>1. 第一种是:instanceof;
>+ 在一个全局域中可以正常返回
>+ 存在两个以上不同的Array构造函数`(多个框架)`,就不能检测出是否是数组
>2. 第二种是:Array.isArray( );
>+ 最终确定是不是数组,而不管他是不是在全局域创建的,`还是有多个域`
>+ 有兼容性支持`IE9+`

### 转换方法

>1. toLocaleString( )
> + 经常返回与valueOf( )和toString( )相同的值`(本地)`
>2. toString( )
>+ 这个在Json里会自执行
>3. valueOf( ); 


```js
var peo = {
     toString : function(){
          return 1;
      },
      toLocaleString : function(){
          return "2"
      }

  }
  var arr = [peo,peo.toLocaleString()];
  alert( arr);//1,2
}
//查看数组里的数据类型
var arr = [1,"20",3]
console.log(typeof arr[1]);//string
```

### Array数组方法

**`join( )`**

>不会改变数组,但返回值会改变数据类型
>如果不给join( )传入任何值,或者给它传入undefined,则使用逗号作为分隔符,IE7会默认给"undefined"分隔符
>如果某一项的值是null或者undefined,那么该值在join( )、toLocaleString（）、toString（）和valueOf方法返回的结果中以空字符串表示

    var arr = [1,"20",3]
    console.log(arr.join("|"));//1|20|3
    console.log(typeof arr.join("|"));//string

**`push()`**
>会改变原数组
>往后面推入项(`数组内`)
> 参数可多写

    var arr = [1];
    console.log(arr.push("2"));//1,2

**`pop()`**
>移除尾部,没有参数,写了没作用
>会返回移除的项,会变数组

    var arr = [1,2,3];
    console.log(arr.pop());//3
    console.log(arr.length);//2

**`shift( )`**
> 移出首位,会改变数组,参数也没作用
> 会返回移除的项

    var arr = [2,1,2,3];
    console.log(arr.shift());//2
    console.log(arr.length);//3

**`unshift()`**
>向头部添加一个或更多元素,并返回长度
>会改变数组
>必须传入参数

    var arr = [5,9,3];
    console.log(arr.unshift(1,2));//5
    console.log(arr);//1,2,5,9,3

**注:**`除了join()外,shift()、unshigt()、push()、pop(),splice(),reverse()`都会改变数组

### `Array( )`操作方法

**`concat()`**

>在数组后面插入,参数可写多个
>不会改变原数组
>不写参数则表示和调用的数组一样,返回值一样

    var arr = [1,2,3,5];
    var a = arr.concat(7,6);
    alert(a);//1,2,3,5,7,6
    alert(arr);//1,2,3,5
    var arr = [1,2,3,4];
    var a = arr.concat();
    alert(a);//1,2,3,4

**`splice()`**
>1. splice(`index,howmany,item1,.....,intmx`)
> + index必需,整数,规定添加/删除项目的索引,**`[`** 可以使用负数( 数组长度和负数相加),`如果是添加,元素会往高位移动` **`]`**
> + howmany必需,要删除的项目数量,如果设置为`0`,则不会删除项目
> + item1,.....,intmx可选,向数组添加的新项目
>2. `splice(添加,删除,替换)`
>3. `splice(位置(从哪儿开始),删除,替换)`
>4. 会改变原数组

```js
//删除
var arr = [1,2,3,4,5];
var a = arr.splice(2,2);
alert(a);//3,4
alert(arr);//1,2,5
//添加
var arr = [1,2,3,4,5];
var a = arr.splice(2,0,"10","20");
alert(arr);//1,2,"10","20",3,4,5
//删除和替换
var arr = [1,2,3,4,5];
var a = arr.splice(2,1,"1");
alert(a);//3
alert(arr);//1,2,1,4,5
```


**`冒泡排序`**
[十大排序方法](https://github.com/hustcc/JS-Sorting-Algorithm)
>性能不太好

```js
<script>
     function quickSort(arr){//5,1
         var len = arr.length;
         for(var i=0;i<len-1;i++){
             for(var j=0;j<len-1-i;j++){
                 if( arr[j]>arr[j+1] ){
                     var temp = arr[j+1];
                     arr[j+1] = arr[j];//引用,并不是赋值
                     arr[j] = temp;
                 }
             }
         }
         return arr;
     }
     var arr = [1,9,10,122,333,555,222,18];
     alert(quickSort(arr));
 </script>
```

**`sort()`**
>sort( )默认是排序是按照字符编码来排序的
>所以如果我们要排序数组中的数字,则要用二分法

```js
<script>
    //按字符排序  
    var str = ['yangzhou','suzhou','nanjin','beijin'];  
console.log(str.sort());//["beijin","nanjin","suzhou","yangzhou"]
</script>
```

**给数组使用sort( )**
>会改变原数组

```js
var arr = [1,2,9,10,7,4];
//升序
arr.sort(function(a,b){
    return a-b;
})
//降序
arr.sort(function(a,b){
    return b-a;
})
console.log(arr);
//也可以这样
function _sort(arr,state){
    if( state === "yes" ){
        arr.sort(function(a,b){
            if( a>b ){
                return 1;
            }else if( a<b ){
                return -1;
            }else{
                return 0;
            }
        })
    }else{
        arr.sort(function(a,b){
            if( a>b ){
                return -1;
            }else if( a<b ){
                return 1;
            }else{
                return 0;
            }
        })
    }
}
/*
* "yes"是升序
* "no"是降序
* 默认降序
* 更新时间: 20171129
* */
```

**`reverse()`颠倒数组**
>把数组颠倒,会改变原数组

```js
var arr = [1,2,9,4,5];
ar a = arr.reverse();
alert(a);//5,4,9,2,1
alert(arr);//5,4,9,2,1
```

**`slice()`**
>接收一个或两个参数,即要返回项的起始的结束位置.
>在`只有一个参数`的情况下,slice( )方法返回从该参数指定位置开始到当前数组末尾的所有项
>如果有两个参数,该方法返回起始和结束位置之间的项`但不包括结束位置的项`
>如果参数是`负数`,则会用参数和数组长度相加
>`不会改变原数组`
>
>- 如果不传参数,则返回调用他的原数组

```js
let a = [1,2,3,4,5];
let b = a.slice();
console.log(b);//[1, 2, 3, 4, 5]
```




```js
<script>
    var colors = ["red", "green", "blue", "yellow", "purple"];
    var colors2 = colors.slice(1);
    var colors3 = colors.slice(1,4);
    alert(colors2);   //green,blue,yellow,purple
    alert(colors3);   //green,blue,yellow
</script>
```

#### Array`位置方法`

**`indexOf()`**

> 指定内容从开头查找数组,找到返回其下标`(第一次出现)`,找不到返回-1,`从位置0开始`

**`lastIndexOf()`**
>指定内容从结尾查找数组,找到返回其下标`(最后一次出现)`,找不到返回-1

```js
<script>
    var numbers = [1,2,3,4,5,4,3,2,1];
    
    alert(numbers.indexOf(4));        //3
    alert(numbers.lastIndexOf(4));    //5
    
    alert(numbers.indexOf(4, 4));     //5
    alert(numbers.lastIndexOf(4, 4)); //3       

    var person = { name: "Nicholas" };
    var people = [{ name: "Nicholas" }];
    var morePeople = [person];
    
    alert(people.indexOf(person));     //-1
    alert(morePeople.indexOf(person)); //0

</script>
```

### 迭代方法`(遍历数组)`

**都传入三个参数,也可只传一个,`不会修改原数组`**

> + every( ): 对数组中的每一项运行给定函数,如果该函数对每一项`都返回true`,则返回true,否则false
> + some( ): 对数组中的每一项运行给定函数,如果该函数对`任意一项返回true`,则返回true
> + filter( ): 对数组中的每一项运行**给定函数条件**,返回该函数会`返回true的项 组成的数组`
> + forEach( ): 对数组中的每一项运行给定函数,这个方法`没有返回值`
> + map( ): 对数组中的每一项运行给定函数,返回每次函数`调用的结果`组成的数组
> +有兼容性,支持`IE9+`

**`语法:arr.xxx(function)(item,index,arr),thisValue)`**
**语法描述**
> + `item  `   ------------ 必须    当前元素的`项`
> + `index` --------------   可选   当前元素的`索引`值
> + `arr` -------------    可选    当前数组`对象`
> + ` thisValue` ---------------     可选   对象作为该执行会掉是使用,传递给函数,`用作"this"的值`

**`forEach()兼容`**

```js
if (!Array.prototype.forEach){
     Array.prototype.forEach = function(fun /*, thisp*/){
         var len = this.length;
         if (typeof fun != "function")
             throw new TypeError();
         var thisp = arguments[1];
         for (var i = 0; i < len; i++){
             if (i in this)
                 fun.call(thisp, this[i], i, this);
         }
     };
 }
```

**例子:`(every())`**

```js
var arr = [1,2,3,4];
var _new = arr.every(function(x){ return x < 10;}) 
// true 都小于10
var _new = arr.every(function(x){ return x%2 == 0;}) 
//false 是不是都是偶数
//示例
var arr = [1,2,3,4,5,6,7,9,8];
var result = arr.every(function(item,index,arr){
    document.write(item+"<i>er</i>"+index+"<span>wu</span>");
});
//有回调函数的情况
var obj = {
   o : function(){
        return console.log(this);//String
	 }
}
function traverse(item,index,arr){
        document.write(item+"<i>er</i>"+index+"<span>wu</span>");
}
var result = arr.forEach(traverse,obj.o.call("hello"));//必须写,不一定要调用
//1er0wu2er1wu3er2wu4er3wu5er4wu6er5wu7er6wu9er7wu8er8wu
console.log(result);//undefined
```

**缩小方法`(求和、求差.....)`**
> + reduce( )和reduceRight( )   都会`迭代数组的所有项`,然后`构建一个最终返回的值`
> + reduce( ) 从第一项开始,而reduceRight( )从数组的最后一项开始,向前遍历到第一项


```js
 /*
        * prev   前一个值(从0开始)
        * cur    下一个值
        * index   当前下标
        * arr     当前的数组对象
        * */
        var arr = [1,2,3,4,5,5];
        var result = arr.reduce(function(prev,cur,index,arr){
            return prev+cur;
        });
        document.write(result);//20
```
#### 获取到数组的最大值
[四种方式](https://blog.csdn.net/web_hwg/article/details/68065587)
- 使用call方法

    let arr = [1,3,2,6,0,3,6,4,8,10,20,65];
    console.log(Math.max.call(...arr));
    //也可以
    //Math.max.call(this/null/Math,...arr);

- 使用apply方法

    Math.max.apply(Math, arr);
    //也可以这样
    //Math.max.apply(this,null, arr);
    //如果不写第一个参数,则返回-Infinity

- 其他请看链接
  