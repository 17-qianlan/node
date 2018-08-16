## BOM

> BOM的核心是window对象,BOM是浏览器的一个实例
> 在网页中定义的任何一个对象、变量和函数，都以window作为其Global对象，因此可以有权访问`parseInt( )`方法
> ECMAScript是JavaScript的核心

**`全局作用域内所有声明的变量、函数都会变成window对象的属性和方法`**
**全局变量不能通过delete操作符删除,而直接在window对象上的定义的属性是可以的**

     var age = "18";
     window.color = "red";
     //alert(delete window.age);//IE8报错
     alert(delete window.color);//IE8报错  其他true
     var o = window.c;//不写window报错
     console.log(o);//undefined

#### 系统对话框

**`alert()`弹窗**
**confirm( )和`alert`差不多,但是可以判断是否被点击,一次可以做一些判断**
**`prompt("输入",默认值)`可以获取到弹窗内填写的信息**

    //prompt
    var str = prompt("您是");
    document.write(str);
    //confirm
     var str = "您确定点击,这是..........";
     //confirm(str);这里不必写
     //判断confirm是否被点击
     if(confirm(str)){
         document.write("其实也没啥");
     }else{
         document.write("你咋这么不皮呢");
     }
     //alert()
     alert("OK");

#### 浏览器信息`(location)`

    <script>
    	//返回URL#后面的
    	console.log(window.location.hash);
    	//返回端口号和服务器名称
    	console.log(window.location.host);//localhost
    	//不带端口号的服务器名称
    	console.log(window.location.hostname);//localhost
    	//完整的URL
    	console.log(window.location.href);
    	//http://localhost/20171207/location.html
    	//返回URL的目录或文件名
    	console.log(window.location.pathname);
    	// /20171207/location.html
    	//返回指定URL中的端口号,如果URL中不包含端口号,则返回空字符串
    	console.log(window.location.port);
    	//空
    	//http协议
    	console.log(window.location.Protocool);
    	//也是空,因为没有使用http
    	//返回URL查询的字符内容?后面的部分
    	console.log(window.location.search);//同样也没有
    	//以上可以通过特定的名称改变后缀,达到更改网址的目的
    	会在浏览器里生成一个历史记录
    </script>

**`assign(),传递一个URL`**
> location.assign( "URL" );
> 下面的和显示的调用assign( )
>  + `window.location(URL);`
>  + ` location.href(URL);`

**window.location.href(URL)`打开一个网址,不会生成新的历史记录`**
**`replace(URL)`打开一个网址,不会生成新的历史记录**
**`reload(),`参数`true`是完全重新加载,`不传参数`是从上一次没有加载完的地方开始加载(F5)**

#### navigator`浏览器信息`

> + window.`navigator.userAgent`  浏览器信息
> + 其他请参考`高级程序设计第三版BOM部分`

    //判断是否是IE
    if ( window.navigator.userAgent.indexOf('MSIE') != -1 ) {
    	alert('我是ie');
    	} else {
    	alert('我不是ie');
    }

#### window对象方法

**`open()`,打开一个新的浏览器窗口或查找一个已命名的窗口**

> + 不写地址,则默认打开一个`新窗口`

    window.open(url,target)
    open(地址默认是空白页面，打开方式默认新窗口) 打开一个新窗口
    _self当前     _bank新开一个窗口
    window.open('http://www.baidu.com', '_self');
    var opener = window.open();//返回值 返回的新开页面的w
    indow对象
    opener.document.body.style.background = 'red';
    关闭window.open()打开的窗口
    window.close();
    opener.close();//可以通过关闭用window.open方法打开的窗口

**`window.close(),关闭window.open()打开的窗口`**用事件触发

**`scrollTo()`滚动到指定坐标,打开就到指定的坐标，否则用时间触发**

    <body style="height: 2000px;">
        <script>
            scrollTo(0,200);
        </script>
    </body>

**`scrollBy(xnum1,ynum2)指定的像素值来滚动内容`,参数必须,不带`px`**

    <body style="height: 2000px;">
        <script>
            document.onclick = function(){
                scrollBy(0,20);//每次向下20
            }
        </script>
    </body>

#### History浏览器历史记录

**`属性`** 

>+ length  返回浏览器历史列表中的 URL 数量。


**`方法`** 
>+ back( )  加载 history 列表中的前一个 URL。 `后退`
>+ forward( )  加载 history 列表中的下一个 URL。 `前进`
>+ go( )  加载 history 列表中的某个具体页面。

#### 定时器

> + 和window.open( )、window.close( )、scrollTo( )、scrollBy( )一样都是window对象
> + 定时器时异步函数
> + 在JS中会先执行**同步操作**才会执行**异步操作,`书写是同步的**

**`setTimeout`,隔多久调用一次,只会调用一次**
> 会默认排序,谁在前序号就在前(**只有setTimeout才有**)
> 在清除时可以直接写序号
> 谁先执行看时间
> 请看这个例子

    setTimeout(function(){
    	console.log("aaaaaaaa");//1
    },0);
   	function fn(){
   		console.log("函数fn里的同步代码1");//2
   		setTimeout(function(){
   			console.log("函数内部的定时器");//3
   		},0);
   		console.log("函数fn里的同步代码2");//4
   	}
   	fn();
   	console.log("3333333333");//5
   	执行顺序为: 2 -> 3 -> 5 -> 1 -> 3

**clearTimeout`清除setTimeOut`**
**`setInterval,多久调用一次,一直会调用`**
**clearInterval`清除setInterval`**
### window常用的对象事件
> + `onload ` 文档加载完毕(要等document/img加载完等)才会执行
> + `onscroll`  滚动的时候(鼠标)
> + `onresize ` 调整尺寸的时候`(浏览器的窗口发生改变)`

    window.onresize = function(e){//要传e,如果是IE要写兼容
    	console.log("我的浏览器窗口在改变,请看");
    }
    window.onscroll = function(e){//e可传可不传
    	console.log("你的鼠标是不是在滚动");
    }

**`onload`拓展**
> + 因为在onload中要等文档或者img等的加载完才会执行,这个效率不是很高
> + 而在jQuery中有一个API只要DOM加载完成,就可以开始加载JS
>   + `$(document).ready(function( ){ })`
>+ 而我们可以仿这样的一个操作

**`onreadystatechange事件`,当文档加载变化时就会触发`没有兼容性`**
> + 这个事件有四个状态:
>     + 当文档的readyState 属性发生变化的时候触发 
>     + readyState 属性返回当前文档的状态（载入中……）。 
>     + 该属性返回以下值: 
>     + uninitialized ­ 还未开始载入 
>     + loading ­ 载入中 
>     + interactive  ­ 已加载，文档与用户可以开始交互 
>     + complete  ­ 载入完成

**DOMContentLoaded事件,`必须要用addEventListener添加`只支持`IE9+`**

    	<script>
    		document.onreadystatechange = function(){//该事件没有兼容性
    			//console.log("OK");
    			console.log(document.readyState);
    			//要用这个事件,才会有complete
    		}
    		function fn(){
    			console.log(document.readyState);
    		}
    		/*------------------一个三目-------------------------*/
    		document.addEventListener?
    		document.addEventListener("DOMContentLoaded",fn,false)
    		:document.attachEvent("DOMContentLoaded",fn);
    		//IE8及以下不会alert()
    		/*-------------------封装--------------------------*/
    		function onready(canback){
    			if( document.addEventListener ){
    				document.addEventListener
    				("DOMContentLoaded",function(){
    					callback();
    					document.removeEventListener
    					("DOMContentLoaded",arguments.callee,false);
    					//执行完后移除该函数
    				},false)
    			}else{//兼容IE低版本
    				if( onreadState === "complete" ){
    				//判断文档是否加载完毕
    					callback();
    					document.detachEvent
    					("onreadystatechange",arguments.callee);
    				}
    			}
    		}
    		调用就window.onready(function(){
    				---------
    		})
    	</script>


