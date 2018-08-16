## Cookie
**可以用来存储数据,进而使它能向电脑上的访问者存储数据**
>1.不同的浏览器存放的cookie位置不一样，也是不能通用的 
>2.cookie的存储是以 域名形式 进行区分的 
>3.cookie的数据可以设置名字的 
>4.一个域名下存放的cookie的个数是有限制的，不同的浏览器存放的个数不一样 
>5.每个cookie存放的内容大小也是有限制的，不同的浏览器存放大小不一样



**`cookie的特点`**
> 需要用到服务器,`(必须)`
> 有大小限制和`时间限制`**(时间限制可以设置),`默认浏览器窗口即过期`**
> 多写不会被覆盖
> **value; value**;`(值与值之间的后者必须要空格)`

**设置cookie**
	
```js
document.cookie = "name=hello";
console.log(document.cookie);
```

- 这个是在窗口关闭后就消失,刷新不会消失,而是保存

**`解码decodeURI`,转码encodeURI**

**`设置过期时间`**

- 如果设置的是昨天或者在现在之前的时间时是会立马过期的

```js
var date = new Date();
date.setDate(date.getDate() + 10);	
//这里是过期时间,通过下面的expires来设置,要必须是字符串时间
document.cookie = "name=hello; expires="+date.toUTCString();
console.log(document.cookie);
//封装过期时间
setCookie({
	age:30,
    name:'二狗',
    love:'逛街',
    sex:'男'
},30);
function setCookie(obj,time){
	var date = new Date();
	date.setDate(date.getDate() + time);
	for(var key in obj){
		document.cookie = key + "=" + obj[key] + "; expires="+ date.toUTCString();
	}
}
```


**`获取cookie`**

```js
//设置cookie
		setCookie({
			age:30,
            name:'二狗',
            love:'逛街',
            sex:'男'
		},30);
		function setCookie(obj,time){
			var date = new Date();
			date.setDate(date.getDate() + time);//过期时间设置
			for(var key in obj){
				document.cookie = key + "=" + encodeURI(obj[key]) + "; expires="+ date.toUTCString();
			}
		}
		//获取cookie
		console.log(getCookie("name","age"));
		function getCookie(){
			var cookie = document.cookie;
			var obj = {};
			for(var key in arguments){
				var patter = "\\b"+arguments[key]+"[^;]+",
					reg = new RegExp(patter),
					val = reg.exec(cookie);
					//console.log(val instanceof Array);//true
					val = val[0].split("=");
					console.log(val);
					val = val[1];
					obj[arguments[key]] = decodeURI(val);
			}
			return obj;
		}
```

**`移除cookie`,亦是完整的cookie封装**

```js
//设置cookie
		setCookie({
			age:30,
            name:'二狗',
            love:'逛街',
            sex:'男'
		},30);
		function setCookie(obj,time){
			var date = new Date();
			date.setDate(date.getDate() + time);//过期时间设置
			for(var key in obj){
				document.cookie = key + "=" + encodeURI(obj[key]) + "; expires="+ date.toUTCString();
			}
		}
		//获取cookie
		console.log(getCookie("name","age"));
		function getCookie(){
			var cookie = document.cookie;
			var obj = {};
			for(var key in arguments){
				var patter = "\\b"+arguments[key]+"[^;]+",
					reg = new RegExp(patter),
					val = reg.exec(cookie);
					//console.log(val instanceof Array);//true
					val = val[0].split("=");
					console.log(val);
					val = val[1];
					obj[arguments[key]] = decodeURI(val);
			}
			return obj;
		}
		//移除cookie
		removeCookie("age")
		function removeCookie(){
			var date = new Date();
			date.setDate(date.getDate() - 1);
			for(var key in arguments){
				document.cookie = arguments[key] + "=" + "; expires = " + date.toUTCString();
			}
		}
```

