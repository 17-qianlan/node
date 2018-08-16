##六大数据类之布尔值`(Boolean)`
**Boolean**
> 1. 有两个值,分别是`true`和`false`
> 2. true和false的首字母`不可大写`,否则浏览器就不能正常解析出这是一个`Boolean`值

**下面是各个数据类型转化为Boolean类型的情况**

| 数据类型      |     true |   false   |
| :--------: | :--------:| :------: |
| Number    |    任何`非`零数值(包括infinity) | 0和`NaN`  |
| string | 任何`非`空字符| `" "`(空字符) |
| Object | 任何对象 | null |
| undefined | 不可转换 | undefined |
| Boolean | true | false | 


**Number转换Boolean**

>+ true转换为`1`;
>+ false转换为`0`;

###逻辑操作符
**逻辑非`(!)`**
>`逻辑非是取反操作`
> + 如果操作数是一个对象,返回false;
> + 如果操作数是一个空字符串,返回true;
> + 如果操作数是一个非空字符串,返回false;
> + 如果操作数是数值0,返回true;
> + 如果操作数是任意非0数值(包括infinity),返回false;
> + 如果操作数是null,返回true;
> + 如果操作数是NaN,返回true;
> + 如果操作数是undefined,返回true;


     <script>
		var a = true;
	    alert(!a);//false
	    alert(!"blue");//false
	    alert(!0);//true
	    alert(!NaN);//true
	    alert(!"");//true
	    alert(!12345);//false
	 </script>

**逻辑与`(&&)`**

| 第一个操作数      |     第二个操作数 |   结果   |
| :--------: | :--------:| :------: |
| true    |   true |  true  |
| true | false | false|
| false | true | false | 
| false | false | false |

>1. 一假即假,全真才为`真`
>2. 只有两个值都为真,才会返回真,`否则`都为假
>3. 如果第一个数为真,则返回第二个操作数

**逻辑或`(||)`**

| 第一个操作数      |     第二个操作数 |   结果   |
| :--------: | :--------:| :------: |
| true    |   true |  true  |
| true | false | true | 
| false | true | true|
|false | false | false | 

> 1. 一真即真,只要有一个操作数为真,就为真
> 2. 如果第一个操作数为真,则返回
>3. 如果第一个操作数为假,则返回第二操作数


    
    


