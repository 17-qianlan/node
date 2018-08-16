##时间`(Date)`和`Math`函数
## 创建时间
> + 指定时间到计算机元年的毫秒数`可用于转化为毫秒数`
>    + obj.getTime( )
>    + Date.UTC( ) 括号内的数年份,`月`和`天`及`小时`还有`分钟`,`秒`都是`从0开始`
> + 按照本地时间输出
>     + toLocaleString( )
> + 输出本地时间的年月日
>     + toLocaleDateString( )
> + 输出本地时 分 秒 时区
>     + toTimeString( )
> +  根据当前时间返回国际标准时间
>    + obj.toUTCString()
> + `注:obj = new Date( );`
> + 返回本地时间与UTC时间相差的分钟数
>   + date.getTimezoneOffset( );

    //获取创建本地时间
	var d = new Date();
	console.log(typeof d);//object
	//设置时间时，参数为字符串的时候，月份不需要处理(在括号里写)
	//当前的时间到计算机元年的毫秒数
	console.log(d.getTime());//1513080279964
	//指定时间到计算机元年的毫秒数
	console.log(Date.UTC(2017,10));//1509494400000
	console.log(Date.UTC(d));//NaN
	console.log(Date.parse(2017,10));//1483228800000
	console.log(Date.parse(d));//1513080279000   
	//和getTime()相差了后三位,Date.parse()的后三位都是0
	//不返回毫秒数
	//按照本地时间输出
	d = d.toLocaleString();
	console.log(d);//2017/12/12 下午8:15:26
	//输出本地时间的年月日
	d = d.toLocaleDateString();
	console.log(d);//2017/12/12
	//输出本地时 分 秒 时区
	d = d.toTimeString();
	console.log(d);//20:18:28 GMT+0800 (中国标准时间)
	// 根据当前日期转化为国际标准时间
	d = d.toUTCString();
	console.log(d);//Tue, 12 Dec 2017 12:44:33 GMT
### 获取具体时间

| 方法     |     描述 |
| :--------: | :--------:|
| getFullYear( )    |   年 |
| `getMonth( )`    |   月`(0-11)` |
| getDate( )    |   天`(1-31)` |
| getDay( )    |   周几(0-6),`星期天为0` |
| `getHours( ) `   |   时 |
| getMinutes( )    |   `分` |
| `getSeconds( ) `   |   秒 |
| getMilliseconds( )    |   `毫秒 `|
| get`UTC`MilliSeconds( ) | 返回UTC日期中的毫秒数|



      var d = new Date(),
		//年
		year = d.getFullYear(),
		//月
		mon = d.getMonth(),
		//星期
		week = d.getDate(),
		//天
		today = d.getDay(),
		//小时
		hour = d.getHours(),
		//分钟
		min = d.getMinutes(),
		//秒
		sec = d.getSeconds(),
		//毫秒
		Mil = d.getMilliseconds();
    	console.log(year,mon,week,today,hour,min,sec,Mil);
    	//2017 11 12 2 20 30 45 353

### 设置时间
> + 设置年份
>        + obj.setFullYear(2020)             传入的值必须是`4位数`
>        + obj.setUTCYear( )                    传入的值必须是`4位数`
>+ 设置月份
>        + setMoth(6)              从0开始,`超过11,则增加年份`
>        + setUTCMoth( )         从0开始,超过`11,则增加年份`,国际标准时间
>+ 设置日份
>        + setDate(20)            ` 1-31`
>        + setUTCDate( )          `1-31 `国际标准时间
>+ 不可以设置星期
>+ 设置时
>        + setHours(10)            `0-23`         超过23 ,`增加天`
>        + setUTCHours( )         `0-23  `     超过23 ,`增加天`, 国际标准时间
>+ 设置分
>        + setMinutes(40)        `0-59 `超过59        `增加小时`
>        + setUTCMinutes( )       超过59         `增加小时`          国际标准时间
>+ 设置秒
>        + setSeconds(15)        ` 0-59` 超过59         `增加分`
>        + setUTCSeconds( )      超过59        ` 增加分`        国际标准时间
>+ 设置毫秒
>        + setMillisSeconds(15)           设置日期中的毫秒数  0-999

**` new Date( 年,月,星期,小时,分钟,秒,毫秒 )  数字形式`**
**` new Date( "年/月/星期/小时:分钟:秒:毫秒" )  字符串形式`**
**以上两个`年`和`月`必须,其他可省**

### 将毫秒转化为`年``月``日``星期``天``小时``分钟``秒`

    var date = new Date();//当前日期
	var date1 = date.getTime();
	var date2 = new Date(year,mon-1,day);//指定日期
	var date3 = date2.getTime();//转化为毫秒数
	var ss= date3-date1;
    var	day = Math.floor(ss/1000/60/60/24),//天
		hours = Math.floor(ss/1000/60/60%24),//小时
		mins = Math.floor(ss/1000/60%60),//分钟
	    secs = Math.floor(ss/1000%60);//秒


**时间补零及转化**

    <div id="box"></div>
		<script>
			var oBox = document.getElementById("box");
			var arr = ['日','一','二','三','四','五','六'];
			function AddTime( obj ){
				return obj>=10?obj:"0"+obj;
			}
			var d = new Date(),
				//年
				year = d.getFullYear(),
				//月
				mon = d.getMonth(),
				//星期
				today = d.getDate(),
				//天
				week = d.getDay(),
				//小时
				hour = d.getHours(),
				//分钟
				min = d.getMinutes(),
				//秒
				sec = d.getSeconds(),
				//毫秒
				Mil = d.getMilliseconds();
				console.log(week);
			oBox.innerHTML += AddTime(year)+"年"+AddTime(mon+1)+"月"+AddTime(today)+"日"+"星期"+arr[week]+AddTime(hour)+"时"+AddTime(min)+"分"+AddTime(sec)+"秒";
		</script>


### Math函数
>+ **随机**：Math.`random`( )*(20-10)+10;随机0-10的数；
>+ **保留小数** : `(Math.random()*(20-10)).toFixed(2)`
>+ **取整**：Math.`round`(num);四舍五入
>+ **向上取整**：Math.`ceil`(num);
>+ **向下取整**：Math.`floor`(num);
>+ **绝对值**：Math.`abs`(num);
>+ **幂**：Math.`pow`(x,y);=x^y
>+ **求根（开方）**：Math.`sqrt`(num);
>+ **返回最大值**：Math.`max`(num1,num2,num3);
>+ **返回最小值**：Math.`min`(num1,num2,num3);
>+ **保留小数（几位）**：num = num.`toFixed`(x);保留x为小数位
>+ **正弦**：Math.`sin`( );
>+ **余弦**：Math.`cos`( );
>+ **π**：Math.`PI`( );
>+ 1弧度=半径等于弧长所需的角度；



