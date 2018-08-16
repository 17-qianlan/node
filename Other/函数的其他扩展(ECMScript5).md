## 函数的其他扩展`(ECMScript5)`
**多态**
> + 根据参数的不同,做出相应的**`反馈`**
> + 用于增加代码的弹性,提升`效率`

如:

    <script>
		var gMap = {
		    show :function () {
		        alert("调用google地图")
		    }
		};
		var bMap = {
		    show : function () {
		        alert("调用baidu地图")
		    }
		};
		var tMap = {
		    show : function () {
		        alert("调用Tencent地图")
		    }
		};
		var temp = function(type){
			if(type.show instanceof Function){
				type.show();
			}
		}
		temp(tMap);
	</script>


### ECMAScript 对象类型
> + 在 ECMAScript 中，所有对象并非同等创建的。 
一般来说，可以创建并使用的对象有三种：` 本地对象` 、 `内置对象` 和 `宿主对象`  、 `自定义对象 `

**本地对象包括：**
> + Object
> + Function
> + Array
> + String
> + Boolean
> + Number
> + Date
> + RegExp
> + Error
> + EvalError
> + RangeError
> + ReferenceError
> + SyntaxError
> + TypeError
> + URIError

**内置对象：**
> + ECMA-262 只定义了两个内置对象，即 `Global`（window） 和 `Math` （它们也是`本地对象`，根据定义，每个内置对象都是本地对象）

**宿主对象:**
> + 所有`非本地`对象`都是`宿主对象（host object），即由 ECMAScript 实现的宿主环境提供的对象。
> + 所有 BOM 和 DOM 对象都是宿主对象

### ECMAScript5 对象的属性方法
**对象属性**
> +  constructor 对`创建对象的函数`的引用（指针）。对于 Object 对象，该指针指向原始的 Object() 函数

**对象方法**
> +  `obj.hasOwnProperty(property) `
obj. hasOwnProperty( name ) 来判断一个属性是否是自有属性，自身属性还是继承原型属 性。必须`用字符串`指定该属性。`返回true 或 false`
> + `obj.isPrototypeOf(object)`
> obj.prototype. isPrototypeOf(  obj  )  判断该对象的原型`是否为xxxxx`。 `返回true
或 false`
>+  `obj.propertyIsEnumerable()`
>obj.propertyIsEnumerable( ‘name’ )  判断对象给定的属性 `是否可枚举` ，**(即是否可用  for...in  语句遍历到,返回true 或 false)**
>+ **getter /setter**  ，函数
>`getter` , `setter` : 返回property的值得方法，值： function( ){ } 或  undefined  默认是 undefined
>+ `__defineGetter__() `, `__defineSetter__()`定义 getter setter函数
>在对象定义后给对象添加getter或setter方法要通过两个特殊的方法
`__defineGetter__ `和 `__defineSetter__ `。这两个函数要求`第一个`是getter`或`setter的名称，以string给出，第二个参数是作为`getter`或`setter`的函数。
>+ ` __lookupGetter__ `,` __lookupSetter__ ` 返回getter setter所定义的函数
> `obj.lookupGetter(sprop)`

    <script>
         function has(){
             this.index = "2";
         }
         has.prototype.name = 999;
         var obj = new has();
         //hasOwnProtyperty
         console.log(obj.hasOwnProperty("index"));//true
         console.log(obj.hasOwnProperty("name"));//false
         //isPrototypeOf
         console.log(has.prototype.isPrototypeOf(obj));//true
         console.log(obj.isPrototypeOf("name"));//false  
         //obj.propertyIsEnumerable()  
         console.log(obj.propertyIsEnumerable("index"));//true
         console.log(obj.propertyIsEnumerable("name"));//false
         //setter和getter
         var obj = {
              age : 10,
              get name(){
                  alert(this.age);
                  return this.age;
              },
              set name(val){
                  alert("val");
                  this.age = val;
              }
          }
          obj.name;//调用get函数   10 
          obj.name = 20;//调用set函数  val
          obj.name;//调用get函数,但被set函数更改   20
          //等价于
          var obj = {
                _age : 10
            }
            //定义get
            obj.__defineGetter__("name",function(){
                alert(this._age);
                return this._age;
            })
            //定义set
            obj.__defineSetter__("name",function(val){
                alert("val");
                this._age = val;

            })
            obj.name;
            obj.name = 20;
            obj.name;
            console.log(obj.__lookupGetter__("name"));
            document.write(obj.__lookupSetter__("name"));
    </script>

### ECMAScript5 Object的新属性方法
**property属性**
 **`property在`JS`中有三个属性`**
 > + 1.writable。该property是否可写。
 > + 2.enumerable。当使用for/in语句时，该property是否会被枚举。
 > + 3.configurable。该property的属性是否可以修改，property是否可以删除。


**Object.defineProperty(O,Prop,descriptor)/`Object.defineProperties(O,descriptors)`**
> + `O`  ——————————–为已有 对象
> + `Prop`  —————————为 属性
> + `descriptor`  —————–为属性 描述符
> + `descriptors`  —————­多个属性 描述符 ？

    <script>
       var obj = {
	       "age" : 10
       };
       Object.defineProperty(obj,"age",{
           writable: true,//表示是否可以在外面重写
           enumerable : true,
           configurable : false,
           value : 20
       });
       obj.age = 30;
       alert(obj.age);//30
    </script>

**Object.defineProperties(O,descriptors)`添加多个属性符`**

    <script>
        var obj = {};
        Object.defineProperties(obj,{
            name:{
                writable : true,
                configurable : true,
                enumerable : true,
                value : "ty"
            },
            age:{
                writable : false,
                value : 19
        }})
        obj.name = "ql";
        obj.age = 21;
        alert(obj.name);//ql
        alert(obj.age);//19
    </script>
**Object.defineProperties中set和get函数的情况**

    <script>
	      function fn(){
	          this.name = "ty"
	      }
	      fn.prototype.age = 19;
	      var obj = new fn();
	      Object.defineProperties(obj,{
	          index : {value : 2},
	          say : {
	              value : 5,
	              writable : true//比须是可写属性,否则set函数没用
	          },
	          //age : {
	              //value : 1, 写value报错
	              //get : function(){
	                 // alert(this.say);
	                  //return this.say;
	             // },
	              //set : function(val){
	                 // alert("val");
	                 // this.say = val
	             // }
	          //}
	          //可改写成,等价于
	          age : {
	              get(){
	                  alert(this.say);
	                  return this.say;
	              },
	              set(val){
	                  alert("val");
	                  this.say = val;//这里的属性必须是可写属性
	              }
	          }
	      });
	      obj.age;//5
	      obj.age = 20;//val
	      obj.age;//20
	      var des = Object.getOwnPropertyDescriptor(obj,"age");
          console.log(des);
          //{enumerable: false, configurable: false, get: ƒ, set: ƒ}
          var des = Object.getOwnPropertyDescriptor(obj,"index");
          console.log(des);
          //{value: 2, writable: false, enumerable: false, configurable: false}
	  </script>

**获取对象的自有的指定的属性描述符**
**`Object.getOwnPropertyDescriptor(O,property)`**
>+ 代码接上面

**获取所有可以枚举的属性名,返回一个数组**
> + 非继承,返回数组

     <script>
        var obj = {
             name : "ql",
             age : 19,
             set : "men"
         }
         var arr = ["1","2","3"];
         console.log(Object.keys(obj));
         //["name", "age", "set"]返回的是一个数组
         console.log(Object.keys(arr));//["1","2","3"]
     </script>

**Object.getOwnPropertyNmaes(obj)返回obj的原有属性,`返回数组`**

    var obj = {age:20,name:'二狗'}
    //console.log( 'age' in obj);//true
    Object.defineProperty(obj,'sex',{
        value:'男',
    });
    var key = Object.getOwnPropertyNames(obj);
    console.log( key );//Array(3)

    var arr = ['A'];
    var key = Object.getOwnPropertyNames(arr);
    console.log( key );//Array(2)

**阻止对象扩展**
**Object.preventExtensions( O )  / `Object.isExtensible()`**
> + Object.preventExtensions( O )   阻止对象拓展 ，即：不能增加新的属性，但是`属性的值仍然可以更改，也可以把属性删除`
> + Object.isExtensible( O ) 用于判断对象是否可拓展

**密封对象**
**` Object.seal(O)`  / Object.isSealed( )**
> + Object.seal( O ) 方法用于把对象密封 ，也就是让对象`既不可以拓展也不可以删除属性`（把每个属性的 configurable 设为false）,`单数属性值仍然可以修改`
> + `Object.isSealed（）` 由于判断对象是否被密封
> + 支持`IE9+`,`不支持Opera`

[参考链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/seal) 
[一些兼容性ES6和ES5](http://kangax.github.io/compat-table/es6/)

    var obj = {
	    prop: function () {},
	    foo: "bar"
	};
	// 可以添加新的属性,已有属性的值可以修改,可以删除
	obj.foo = "baz";
	obj.lumpy = "woof";
	delete obj.prop;
	var o = Object.seal(obj);
	assert(o === obj);
	assert(Object.isSealed(obj) === true);
	// 仍然可以修改密封对象上的属性的值.
	obj.foo = "quux";
	// 但你不能把一个数据属性重定义成访问器属性.
	Object.defineProperty(obj, "foo", {
		 get: function() { 
			 return "g"; 
		 } 
	 }); // 抛出TypeError异常
	// 现在,任何属性值以外的修改操作都会失败.
	obj.quaxxor = "the friendly duck"; // 静默失败,新属性没有成功添加
	delete obj.foo; // 静默失败,属性没有删除成功
	// ...在严格模式中,会抛出TypeError异常
	function fail() {
	  "use strict";
	  delete obj.foo; // 抛出TypeError异常
	  obj.sparky = "arf"; // 抛出TypeError异常
	}
	fail();	
	// 使用Object.defineProperty方法同样会抛出异常
	Object.defineProperty(obj, "ohai", { value: 17 }); // 抛出TypeError异常
	Object.defineProperty(obj, "foo", { value: "eit" }); // 成功将原有值改变

**完全冻结**
**`Object.freeze(O) ` / Object.isFrozen( )**
> + 终极神器，完全冻结对象 ，**在seal的基础上**，`属性值也不可以修改`（每个属性wirtable也被设为 false ）