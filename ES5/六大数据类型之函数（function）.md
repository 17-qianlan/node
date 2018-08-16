##六大数据类型之函数（function）
[call，apply，bind的参考链接](www.cnblogs.com/pssp/p/5215621.html)
[this指向的参考链接](http://www.cnblogs.com/pssp/p/5216085.html)
[匿名函数及匿名函数的自执行参考链接](http://www.cnblogs.com/pssp/p/5216668.html)
####作用域
>作用域:脚本的有效范围、作用范围。
>分两类：`全局（script`）和`局部（function）`

**全局作用域**
>直接定义在script标签下的`变量`以及函数、他们都作用在一个域，`全局作用域`
>直接定义在script标下的变量称之为全局`变量`，script表签下的函数，称之为`全局函数`
>全局变量及函数都是`window的一个属性`，都能通过`window.变量名`访问
>**ES5所有的全局变量都默认挂载在顶层对象上,否则会污染这个全局环境**


	  <script>
		  var a = 123;
		  alert(widown.a);//123
		  function abc(){}
		  alert(window.abc());//function abc(){}
		  (function(){
				....//在这里写,就不会把所有的变量都挂载在顶层对象上
           })()
	  </script>


	  
**局部（function）作用域**
>任何一个function( ){ }，都会开启一个`局部作用域`
>定义在function( ){ }内部的变量称之为`局部变量`

    var a = 20;
    function fn(){
	    alert(a);//20
    }
    window.fn();
>作用域链：局部作用域内部可以访问`父级作用域变量及全局作用域变量`，也可以访问`父级`的函数，及全局函数`（从里往外找）`

    var a = 10;
    (function(){
	    alert(a);//10
    })()

>局部的变量会`覆盖父级（全局）变量`，函数亦如此
>变量的复制

    //变量的复制
    var num = 1；
    var num1 = num；
    //一个经典的例子
    function setName(obj){
	    obj.name = "hello";
	    //这里的开始的obj其实是一个新对象，所以他不会改变setName的值
	    obj = new Object();
	    obj.name = "hi";
    }
    var person = new Object();//重新赋值
    setName(person);
    alert(person.name);//hello


####Javascript解析
>Javascript解析即代码读取过程
>`至上而下`,`从左至右`(**运算时注意优先级**)
>`还会预解析`：正式`解析前的工作`，预解析过程会出现`变量提升`，`函数提升`
>`变量提升`：在作用域内声明的变量会被提升到作用域的顶部，且对其赋值`undefined`，这个过程就是变量提升
>不管是在判断里是`true还是false`都会发生`预解析`

    (function (){
	    alert(b);//undefined
	    var b = 10;
    })()
    //报错
    (function (){
          alert(b);
    })()
    var b = 10;
    //提升了d,并对其赋值undefined
    function ab(){
        console.log(d);
    }
    ab();
    var d = 10;//undefined

>函数提升：在作用域内的俄函数定义函数会被提升到`作用域内的顶部`，其值为其函数本身，这个过程为函数提升
>`不会提升的函数`：在作用域内的函数表达式`函数`不会被提升到作用域的`顶部`
>重名的变量只留一个，`var和函数重名函数优先`
#### 函数的形参和同名的var变量
- 在形参(a)和实参(b)实际是一个`隐式`的var a = b,而且在函数内部的`var后面才会执行`,所以他会`覆盖var提升的部分`,从而输出的值就是`a = b`;

    var num = 5;
    function fn(num){
        alert(num);//5
        var num = 10;
        alert(num);//10
    }
    fn(num);
####函数声明与函数表达式
>解析器在向执行环境中加载数据时，对函数声明和函数表达式`并非一视同仁`
>解析器会率先读取函数声明，至于函数表达式，则必须等到解析器`执行`到他所在的代码，才会真正被解析

####函数的垃圾回收
>JavaScript具有`自动`垃圾收集机制，执行环境会负责管理代码执行过程中使用的内存
> **JS底层垃圾回收机制**: `当整个环境里,都不在对这个对象/数据的引用时,那么它就会被垃圾回收`,先编译再执行;
>JavaScript中函数如果没有return，则会在函数执行完之后就被垃圾回收，随后赋值`undefined`
>全局变量不要写太多,因为全局变量**不会**被垃圾回收

####函数执行
>函数执行的方式有：`自执行`，`事件触发`（点击事件）

**有名函数的执行**

    function a( ){
	    alert(this);//window
    }
    a();

**匿名函数的执行**

    //常见的两种
    （function（）{
	    alert（this）；//window
    ）（）
	（function（）{ 
		alert（this）；//window
	}（））

**事件触发**

    oBox.onclick = function(){
	    alert(this);//oBox
    }

>可以把函数理解成一个·指针·，但我们要去访问这个指针时，则不能让函数执行，要把他的`圆括号`给去掉，是`引用`而`非赋值`

####return
>一般函数在执行完以后都会被垃圾回收随后`赋值undefined`，但使用`return`可以把函数执行完成后的`某一个`值保留下来
>一旦被return后，后面的代码将`不会`再执行
>函数中return的`默认值`是`undefined`
>`return`返回可以是任何类型,`但注意如果是多个值用+拼接则只会返回最后一个,但都会执行`

    var a;
	function fn( ){
	   a = 10;
	   return a;
	}
	//不return,就出来的是undefined
    alert(fn( ));//10

####函数传参
>函数参数有`形参`和`实参`
>其中形参是在`函数`括号内的参数
>实参是在`自执行`括号内的参数



    function ab(形参){}
    ab(实参)；

>不定参；即函数参数的个数不确定
>`arguments`所有函数都`存在arguments`，是所有实参的集合，arguments是一个`类数组`，有长度，`arguments.length`可以返回个数

    function a(){
	    console.log(arguments[0]+arguments[1]+........);
	    alert（arguments.length）；
    }
    a(1,2,3,4)；

**callee**
>`arguments.callee`代表这个函数

     function a(){
             var c = arguments.callee;
             console.log(c);    
     }
     a(10);

**caller**
>这个函数保存着当前调用函数的值，如果是在`全局作用域`中调用当前函数的值，则为null
>`谁调用及指向谁`

**注:**现在的arguments.callee和caller已经很少用了,因为它代表这整个函数,调用时会重新创建,影响浏览器性能
[arguments.callee的一些使用参考](https://www.cnblogs.com/lijinwen/p/5727550.html)

####闭包(return)
- 特点
>`至少要有两个函数,函数嵌套函数`
>`return内部函数`
>内部函数使用的外部函数的变量/参数**(必须,否则不是闭包)**
>`如果外部函数内的变量没有被内部函数调用,则会返回undefined(被垃圾回收)`

    var a = 5;
	function fn(){
		var a = 10;
		alert(a);
		function b(){
			a++;
			alert(a);
		}
		return b;
	}
	var c = fn(); //10
	c();//11
	fn()();//10 11
	c()//12
- 作用
> 内部函数使用的外部函数的变量/参数,会永久被**保存**下来

    function fn(){
	       var n = 1;
	       function fn2(){
	           n++;
	           console.log(n);
	       }
	       return fn2;
	   }
	   var fn3 = fn();
	   fn3();//2
	   fn3();//3
	   fn3();//4
	   fn3();//5
	   fn3();//6
	   fn3();//7
	   //...... 每次加1
	}

##### 一个经典的例子
- - fn和最外层的函数**`不是闭包关系`**(因为`fn`没有调用最外层函数的**a**)

    (function () {
         var a = 5;
         function fn() {
             var a = 10;
             console.log(a);
             function b() {
                 a++;
                 console.log(a);
                 function c() {
                     a++;
                     console.log(a);
                 }
                 return c;
             }
             return b; 
         }
         var b = fn();
         b(); //a+1= 10 + 1 = 11
         var c = b();//11+1=12
         c(); //12+1=13
         b(); //13+1=14
         fn()(); 
         b();//14+1=15
     })();

####递归

    //一般我们计算5!
    //5! = 5*4*3*2*1 = 120;
    //前面是递,后面是归,这个过程就是递归
    5! = 5*4!
    4! = 4*3!
    3! = 3*2!
    2! = 2*1!
    //我们可以:
    function sum(num){
	    if(num<=1){
		    return 1;
	    }else{
		    return num*arguments.callee(num-1);
	    }
    }
    alert(sum(5));//120
####this指向

    widow.o = "o";
    (function(){
	    return this.o;//"o"
    })()
> 在函数内的this默认是指向`window`的
> 但是事件函数内就是指向这个`事件函数`的事件名

    oBox.onclick = function(){
	    alert(this);//oBox
    }
    //这里的this会报错
    var o = {
		user = "浅蓝";
		alert(this.user);//undefined
	}
	//这样写
	function a(){
        let user = "浅蓝";
        console.log(this.user);
    }
    a();//undefined
	window.o.fn();
    var o = {
	    user:"浅蓝",
	    fn:function(){
	        console.log(this.user); //浅蓝
	    }
	}
	window.o.fn();
	var o = {
	    a:10,
	    b:{
	        // a:12,
	        fn:function(){
	            console.log(this.a); //undefined
	        }
	    }
	}
	o.b.fn();
	var o = {
	    a:10,
	    b:{
	        a:12,
	        fn:function(){
	            console.log(this.a); //undefined
	            console.log(this); //window
	        }
	    }
	}
	var j = o.b.fn;//j是全局,所以是window
	j();
  >1. 如果一个函数中有this，这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，this指向的也只是它上一级的对象,在哪儿调用,`this就指向这个域`
  >2. 如果一个函数中有this，这个函数有被上一级的对象所调用，那么this指向的就是上一级的对象。
  >3. 如果一个函数中有this，但是它没有被上一级的对象所调用，那么this指向的就是window，这里需要说明的是在js的`严格版`中this指向的不是window，

**我们可以通过call,apply,bind的方法改变this的指向,使它指向我们所希望的对象**  

    //call,在函数里边调用参数时,必须一个个写,不可以传一个数组后类数组(arguments)
    function a(){
	  alert(this);  
	  //alert(this,num1,num2......)
    }
    a.call("hello");//hello
    //apply作用和call方法差不多,但是里边要写一个数组或类数组,
    function a(){
	    alert(this);
	    //alert(this,arguments/[])所有的参数都放在里边
    }
    a.apply("hello");//hello

**bind( )**
>都用在函数的创建中`(被动执行)`

`适用于匿名函数`

    //参数都是和call方法一样,要一个一个写
    var fn = function (a,b ){//函数表达式
		console.log(this,a,b);
	}.bind('hello',1,2);//"hello必写","this(没写就没必要这样写了)也要写"否则是NaN
	fn();
	//第二种方式
	var fn = function (a,b ){
		console.log(this,a,b);
	}.bind('hello');
	fn(1,2);
	///第三种
	obj.onclick = function(){
	
	}.bind()
	box.onclick = function(){
         console.log(this);//window,不然他就是指向事件函数的
	}.bind(window);
    
**有名函数**

    function fn(){
		console.log(this);//hello,不然是window
	}
	fn.bind('hello')();

**函数自执行**

    //1
    (function abc(){
		console.log(this);
	}.bind('hello')());
	//2
	(function abc(){
		console.log(this);
	}.bind('hello'))()
	//3
	(function abc(){
		console.log(this);
	}).bind('hello')();
	//都写在最后一对圆括号之前写bind方法  匿名函数也相似
	(function (){
         alert(this);//this
    }).bind("this")()

### bind call apply
[链接](https://juejin.im/post/59bfe84351882531b730bac2)


