## ES6(let,const,global)
### let命令

#### 基本用法

```
跟使用es5的var一样
```
#### let与var的区别
> 1. `不存在`变量提升
> 1. var会`存在`变量提升现象，
> 1.而` let`和`const`则不会有这种情况
#### 暂时性死区  简称 TDZ 
> 1. 只要块级作用域内`存在let命令`，它所声明的变量就“**绑定”**（binding）这个区域，不再受外部的影响。
> 2. 暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，**只有等到声明变量的那一行代码出现，`才可以`获取和使用该变量。**
> 3. 即使使用typeof也会报错

    var tmp = 123;
    
    if (true) {
      tmp = 'abc'; // ReferenceError
      let tmp;
    }
    typeof x; // ReferenceError
	let x;

- ES6 明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。**凡是在`声明之前就使用`这些变量，就会报错。**
- 不允许重复申明

- let不允许在相同作用域内，重复声明同一个变量。

    // 报错
    function func() {
      let a = 10;
      var a = 1;
    }
    
    // 报错
    function func() {
      let a = 10;
      let a = 1;
    }

- 因此，不能在函数内部重新声明参数。

    function func(arg) {
      let arg; // 报错
    }
    
    function func(arg) {
      {
        let arg; // 不报错
      }
    }

#### do表达式
- 本质上,块级作用于是一个语句,将多个操作封装在一起,没有返回值

    {
	    let t = f();
	    t  =  t * t + 1;
	}
- 除了使t变为全局变量外,否则我们得不到t的值
- 但是有一个提案,可以拿到t的值,那就是使用do表达式

    let x = do {
	    let t = f();
	    t  =  t * t + 1;
	}

### 块级作用域

#### 为什么需要块作用域

- ES5 只有全局作用域和函数作用域，**没有块级作用域**，这带来很多不合理的场景。

- 第一种场景，**内层变量可能会覆盖外层变量**。

    var tmp = 123;
    
    function f() {
      console.log(tmp);
      if (false) {
        var tmp = 'hello world';
      }
    }
    
    f(); // undefined

- 第二种场景，**用来计数的循环变量泄露为全局变量**。

    var s = 'hello';
    for (var i = 0; i < s.length; i++) {  
      console.log(s[i]);
    }
    console.log(i); // 5

- ES6 的块级作用域 

    function f1() {
      let n = 5;
      if (true) {
        let n = 10;
      }
      console.log(n); // 5
    }

- ES6 允许块级作用域的**任意嵌套**。

    {{{{{let insane = 'Hello World'}}}}};

- 无法使用外层变量

    {{{{
	  {let insane = 'Hello World'}
	  console.log(insane); // 报错
	}}}};

块级作用域的出现，实际上使得获得广泛应用的立即`(匿名)`执行函数表达式`(IIFE)`不再必要了。

    // IIFE 写法
    (function () {
      var tmp = ...;
      ...
    }());
    
    // 块级作用域写法
    {
      let tmp = ...;
      ...
    }

块级作用域不返回值，除非t是全局变量。
#### 块级作用域与函数声明
- 要注意函数会在预解析中会提升
- ES5 规定，函数只能在`顶层作用域`和`函数作用域`之中声明，`不能在块级作用域声明`
- ES6 `引入了块级作用域`，明确允许在块级作用域之中声明函数。

	  // 情况一
	if (true) {
	  function f() {}
	}
	// 情况二
	try {
	  function f() {}
	} catch(e) {
	  // ...
	}
	//为了兼容以前的旧代码，还是支持在块级作用域之中声明函数，因此上面两种情况实际都能运行，不会报错。
	

- ES6 规定，块级作用域之中，**函数声明语句的行为类似于let**，在块级作用域之外`不可引用`

    //ES6环境
	function f() { console.log('I am outside!'); }
	(function () {
	  if (false) {
	    // 重复声明一次函数f
	    function f() { console.log('I am inside!'); }
	  }
	  f();//报错
	}());
	
	//相当于执行
	
	// 浏览器的 ES6 环境
	function f() { console.log('I am outside!'); }
	(function () {
	  var f = undefined;
	  if (false) {
	    function f() { console.log('I am inside!'); }
	  }
	
	  f();
	}());
	// Uncaught TypeError: f is not a function

- 考虑到环境导致的行为`差异太大`，应该`避免`在块级作用域内`声明函数`。
- 如果确实需要，也应该**写成函数表达式**，而不是**函数声明语句**

    //函数声明语句
	{
	  let a = 'secret';
	  function f() {
	    return a;
	  }
	}
	
	// 函数表达式
	{
	  let a = 'secret';
	  let f = function () {
	    return a;
	  };
	}

### const
#### 特点
- const声明一个只读的常量。
- const除了以下两点与let不同外，其他特性均与let相同:

```
1. const一旦声明变量，就必须立即初始化，不能留到以后赋值。
2. 一旦声明，常量的值就不能改变。
```

#### 本质
- const限定的是赋值行为。
- 也就是说

    const a = 1;
    a = 2;//报错
    const arr = [];
    arr.push(1) //[1] 
    //在声明引用型数据为常量时，const保存的是变量的指针，只要保证指针不变就不会保存。下面的行为就会报错
    arr = [];//报错 因为是赋值行为。变量arr保存的指针改变了。
    - 如果真的想将对象冻结，应该使用Object.freeze方法。

```
const foo = Object.freeze({});
// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;
上面代码中，常量foo指向一个冻结的对象，所以添加新属性不起作用，严格模式时还会报错。
```

- 除了将对象本身冻结，对象的属性也应该冻结。下面是一个将对象彻底冻结的函数。
```
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
 };
```
### 顶层对象属性与全局变量

#### 顶层对象
##### 特点
- 在浏览器环境指的是window对象，在 Node 指的是global对象。
- **ES5 之中，顶层对象的属性与全局变量是等价的**。

    window.a = 1;
    a // 1
    a = 2;
    window.a // 2

- 顶层对象的属性与全局变量挂钩，被认为是 JavaScript 语言最大的设计败笔之一。
- 为了解决这个问题，es6引入的let 、const和class声明的全局变量**不再属于顶层对象**的属性。
- 而同时为了**向下兼容**，var和function声明的变量依然属于全局对象的属性

    var a = 1;
    window.a // 1
    
    let b = 1;
    window.b // undefined

### global 对象
 - ES5 的顶层对象，本身也是一个问题，因为它在各种实现里面是不统一的。
- **表现如下:** 

```
1. 浏览器里面，顶层对象是window，但 Node 和 Web Worker 没有window。
2. 浏览器和 Web Worker 里面，self也指向顶层对象，但是 Node 没有self。
3. Node 里面，顶层对象是global，但其他环境都不支持。
4. 同一段代码为了能够在各种环境，都能取到顶层对象，现在一般是使用this变量，但是有局限性。
```

- 全局环境中，this会返回顶层对象。但是，Node 模块和 ES6 模块中，**this返回的是当前模块**。
- 函数里面的this，如果函数不是作为对象的方法运行，而是单纯作为函数运行，`this会指向顶层对象`。但是，**严格模式下，这时this会返回undefined。**

```
不管是严格模式，还是普通模式，new Function('return this')()，总是会返回全局对象。但是，如果浏览器用了 CSP（Content Security Policy，内容安全政策），那么eval、new Function这些方法都可能无法使用。
```

综上所述，很难找到一种方法，可以在所有情况下，都取到顶层对象。下面是**两种勉强可以使用的方法**。

```
// 方法一
(typeof window !== 'undefined'
   ? window
   : (typeof process === 'object' &&
      typeof require === 'function' &&
      typeof global === 'object')
     ? global
     : this);

// 方法二
var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};
```
#### 提案

- 现在有一个提案，在语言标准的层面，引**入global作为顶层对象**。也就是说，在所有环境下，`global都是存在的，都可以从它拿到顶层对象。`
- **垫片库system.global**模拟了这个提案，可以在所有环境拿到global。

```
// CommonJS 的写法
require('system.global/shim')();

// ES6 模块的写法
import shim from 'system.global/shim'; shim();
上面代码可以保证各种环境里面，global对象都是存在的。

// CommonJS 的写法
var global = require('system.global')();

// ES6 模块的写法
import getGlobal from 'system.global';
const global = getGlobal();
上面代码将顶层对象放入变量global。
```
### 总结

    let 命令
            let 作用域作用
                   ---作用域代码块
            不存在变量提升
                   ---先声明在使用
            暂时性死区
                    --不可重复定义
       const 常量，跟let基本一致，
            let 作用域作用
                    ---作用域代码块
            需定义即赋值,不可修改值            
            不存在变量提升
                    ---先声明在使用
            暂时性死区
                    --不可重复定义
        块作用域代替自执行函数

        数组的解构赋值
            let [x = 20] = [];
            let [x = fn()] = [];
        数组解构默认值
            --严格的（===）undefined,才能赋值默认值

        对象的解构赋值
            a let {age} = {age:123};
        b let {age,index} = {age:123,name:'二狗'};

            C 对象解构赋值本质
            D 变量名与属性名不一致情况
     let {age:bar,index} = {age:888,index:666};

            e 嵌套赋值

        对象解构默认值
            let {index,age} = {};
            let {index = 123,age =20} = {}
            let {message:msg = 123} = {};


        函数参数的解构赋值
            数组参数
                function fn([x,y])

            对象参数
                function fn({index,age})



            默认值

                数组变量默认值
                数组传递undefined默认值
                数组传递有值默认值

                对象变量默认值
                对象传递undefined 默认值
                对象传递有值默认值

