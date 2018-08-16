##六大数据类型之`typeof`和加减乘除
####typeof
>可以用typeof来检测数据类型

如:

    var a = 1;
    console.log(typeof a);//number
    var str = "hello";
    console.log(typeof str);//string
    function fn(){
        alert("ok");
    }
     console.log(typeof fn);//function
     var obj = {
         name : "hello",
         age : 19
     }
     console.log(typeof obj);//object
     console.log(typeof true);//boolean
     var c = undefined;
     console.log(c);
     var d = 100-"px";
     console.log(d);//NaN
     console.log(typeof a);//number

#####+ - * /( 加 减 乘 除 )
>+ `+`可以表示`拼接`和`相加`;不会转化本身的数据类型
>拼接有字符串的拼接(主要)
>+ `-` 在浏览器里减号会`尽量转化`为number类型,不能转化则输出NaN
>+ `*` 在浏览器里乘号会`尽量转化`为number类型,不能转化则输出NaN
>+ `/` 在浏览器里减号会`尽量转化`为number类型,不能转化则输出NaN

如:

    var d = 100-"px";
    console.log(d);//NaN
    var splice = "s"+"h"
    console.log(splice);//sh
    var sp = 100+"px";
    console.log(sp);//100px
    var sp1 = 100 + "5";
    console.log(sp1);//1005
    var change = 100 * "5";
    console.log(change);//500

**乘法`*`**
> + 如果操作数都是数值,则执行常规操作,否则就会尽量把数值转化为Number类型,不行就返回NaN
> + Infinity与0相乘,则返回NaN
> + Infinity与非0数值相乘,则结果是Infinity或-Infinity,取决于有符号操作数的符号
> + Infinity与Infinity相乘,则结果是infinity

**除法`/`**
> + 如果操作数都是数值,则执行常规操作,否则就会尽量把数值转化为Number类型,不行就返回NaN
> + 0与0相乘,则返回NaN
> + 如果是`非零`的有限数被`零`除,则结果是Infinity或-Infinity,取决于有符号操作数的符号
> + 如果是Infinity被任何非零数值除,则结果是Infinity或-Infinity,取决于有符号操作数的符号


**求模`(%)`**
> + 如果操作数都是数值,执行常规的除法计算,返回除得的余数
> + 如果被除数是无穷大值而除数是有限大的数值,则结果是NaN
> + 如果被除数是有限大的数值而除数是零,则结果是NaN
> + 如果是 Infinity除Infinity,则结果是NaN
> + 如果被除数是有限大的数值而除数是无穷大的数值,则结果是被除数
> + 如果被除数是零,则结果是零
> + 也会调用Number( )

**加法`(+)`**
> + 如果有一个操作数是NaN,则结果是NaN
> + 如果是infinity 加 Infinity,则结果是Infinity
> + 如果是-Infinity 加 -Infinity ,则结果是-Infinity
> + infinity 加 - infinity,则结果是NaN
> + +0 加+0 ,结果是+0
> + -0 加 +0,结果是+0
> + -0 加 -0,结果是+0
> + 如果一个是字符串,另一个是其他,则会把不是字符串的转化字符串并拼接起来

**减法`(-)`**
> + 如果有一个操作数是NaN,则结果是NaN
> + 如果是infinity 减 Infinity,则结果是NaN
> + 如果是-Infinity 减 -Infinity ,则结果是NaN
> + infinity 减 - infinity,则结果是Infinity
> + -Infinity 减 Infinity ,则结果是-Infinity
> + +0 减+0 ,结果是+0
> + +0 减 -0,结果是-0
> + -0 加 -0,结果是+0
> + 会调用Number( ),valueOf( ),toString( )

**注: `valueOf( )主要用于对象,然后返回原对象,还可以返回Boolean原对象的值`**
[valueOf( )使用参考链接](https://www.cnblogs.com/xiaohuochai/p/5560276.html)
**关系操作符`( > < = )`**
>+ == 会转化, 调用Number( )
>+ === 只会比较`(值和数据类型)`只有全部一样才会返回true
>+ >和< 会在比较字符串值`(两个)`时,则比较的是编码值,会调用Number( ),有对象的方法,则调用valueOf( ),没有调用toString( )

| 表达式      |     值 |   表达式   |   值   |
| :--------: | :--------:| :------: |:------: |
| null == undefined    |   true |  true == 1  | true |
| "NaN" == NaN | false| true == 2 | false |
| 5 == NaN | false | undefined == 0 | false |
| NaN == NaN | false | null == 0 | false |
| NaN != NaN | true | "5" == 5 | true |
| false == 0 | true| null ==== undefined | false | 


**简写**

|表达式    |     可简写为 | 
| :--------: | :--------:|
| a + b   |   a += b |
| a * b | a *= b |
| a - b | a -= b |
| a / b | a /= b |
| a % b | a %= b |

**变量自增**

| 表达式      |     释义 |  
| :--------: | :--------:|
| `--` num    |   先自减再赋值 |
| num`--` | 先赋值再自减 | 
| ++num | 先自增在赋值 | 
| num++ | 先赋值再自增 | 


