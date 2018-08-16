## 面向切面编程AOP

### 概念

- 在函数执行前或后要做一些事情
- 执行了
- AOP主要实现的目的是针对业务处理过程中的切面进行提取，它所面对的是处理过程中的某个步骤或阶段，以获得逻辑过程中各部分之间低耦合性的隔离效果 
- 比如我们最常见的就是日志记录了，举个例子，我们现在提供一个查询学生信息的服务，但是我们希望记录有谁进行了这个查询
- AOP的编程，好像就是把我们在某个方面的功能提出来与一批对象进行隔离，这样与一批对象之间降低了耦合性，可以就某个功能进行编程
- 前置 before

```js
Function.prototype.before = function(){}
```



- 后置after

```js
Function.prototype.after = function(){}
```

- 执行顺序
- - 前置  => 本体 => 后置
- 可以一直前置或一直后置,优先级会越来越高

- 环绕around



- 案例
- - 下面这个例子的第二个参数不要管他,没有传值

```js
Function.prototype.before = function( beforeFn , thisObj ){
    let This = this;
    return function(...rest){
        //先执行参数 表示的前置函数
        beforeFn.apply(thisObj);

        //再执行本体
        return This.apply(thisObj,rest);
    }
};
Function.prototype.after = function(afterFn , thisObj){
    let This = this;//这个this指向的是上面的before(前置函数)
    return function(...rest){
        //先执行本体
        let rt = This.apply(thisObj,rest);
		console.log(1);//这里最先调用
        //再执行后置函数
        afterFn.apply(thisObj);

        return rt;
    };
};

function dachui(a,b){
    console.log("大锤本体" ,a,b);
    return "这是大锤的返回值。";
}

function b(){
    console.log("这是一个前置的动作01。");
}
function a(){
    console.log("这是一个后置的动作01。");
}

/*dachui = dachui.before(b).before(function(){
                console.log("这是更前一个的前置动作。");
            });*/
dachui = dachui.before(b).after(a);

console.log( dachui(1, 2) );


/*function goudan(x,y){
                console.log("狗蛋执行了！",x,y);
                return "阿飞";
            }

            //传入 实际的 前置动作函数
            goudan = goudan.before( function(){
                console.log("狗蛋执行 之前 需要执行的内容")
            } );

            console.log( goudan(2, 5) );*/


/*function x(a,b){
                console.log("狗蛋本体",a,b);
            }
            x = x.after(function(){
                console.log("狗蛋执行 之后 需要执行的内容")
            });

            x(4,5);*/


/*let obj = {
                xx(a,b){
                    console.log(this);
                    console.log(a);
                    console.log(b);
                }
            };

            obj.xx = obj.xx.before(function(){
                console.log("前置动作！");
            },obj);

            obj.xx(1,2);*/

/*function goudan(){
                console.log("狗蛋执行了！");
            }

            function before(){
                console.log("狗蛋执行 之前 需要执行的内容")
            }

            function after(){
                console.log("狗蛋执行 之后 需要执行的内容")
            }


            before();
            goudan();
            after();*/


```



### AOP的应用实例

- 用AOP装饰函数的技巧在实际开发中非常有用.不论是业务代码的编写,还是在框架层面
- 我们都可以把行为依照职责分成粒度更细的函数,随后通过装饰把它们合并在一起
- 这有助于我们编写一个松耦合和高复用性的系统.

### 链接

-  https://www.jianshu.com/p/e7f32464a8ab

### 例子

- 用AOP动态改变函数的参数

观察`Function.prototype.before`方法:

```js
Function.prototype.before=function(beforefn){
    var __self=this;
    //保存原函数的引用
    return function(){
    //返回包含了原函数和新函数的"代理"函数
        beforefn.apply(this,arguments);//(1)
    //执行新函数,且保证this不被劫持,新函数接受的参数
    //也会被原封不动地传入原函数,新函数在原函数之前执行
        return __self.apply(this,arguments);//(2)
    //执行原函数并返回原函数的执行结果
    //并且保证this不被劫持
    }
}
```

从这段代码的(1)处和(2)处可以看到,`beforefn`和原函数`__self`共用一组参数列表arguments,当我们在`beforefn`的函数体内改变arguments的时候,原函数__self接收的参数列表自然也会变化.
 下面的例子展示了如何通过`Function.prototype.before`方法给函数`func`的参数param动态地添加属性b:

```js
Function.prototype.before = function(beforefn) {
    var __self = this;
    //保存原函数的引用
    return function() {
        //返回包含了原函数和新函数的"代理"函数
        beforefn.apply(this, arguments); //(1)
        //执行新函数,且保证this不被劫持,新函数接受的参数
        //也会被原封不动地传入原函数,新函数在原函数之前执行
        return __self.apply(this, arguments); //(2)
        //执行原函数并返回原函数的执行结果
        //并且保证this不被劫持
    }
}
var func = function(param) {
    console.log(param);
}
func = func.before(function(param) {
    param.b = 'b';
})
func({a: "a"}); //{a:"a",b:"b"}
```

 

 

 

 

 

 

 

 

 

