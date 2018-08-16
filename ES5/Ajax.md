## Ajax
[TOC]
### Ajax概述
**AJAX =  `Asynchronous JavaScript and XML` 异步的  `JavaScript`  和`XML` **
**`功能：在不刷新页面的情况下，实现与后台的数据交互`**
> + Ajax不是新的编程语言,而是一种使用现用标准的新方法
> + Ajax技术核心是`XMLHttpRequest`对象(**简称XHR**)

 + `运行在服务器`
 + `不能跨域`,跨域一般是后端来解决
 + 不可以在中文路径下运行

### XMLHttpRequest对象方法
- **创建XNLHttpRequest对象**

 > + 创建 new XMLHttpRequest
 > > IE 6以下版本`new ActiveXObject("Msxml2.XMLHttp.3.0")`或`new ActiveXObject("Msxml2.XMLHTTP.6.0")`

#### post的请求头
- **Open(`''`, ` method`,`url`,`asynch`,`[username]`,`[password]`)**

> + 指定和服务器端交互的HTTP方法,URL地址,即其他请求信息`(一共三个参数)`;
> + `Method:http`请求方法,一般使用"`GET`","`POST`"
> + url : 请求的服务器地址;
> + `asynch : `是否采用异步方法,`true为异步`,`false为同步`
> + 后边两个`可以不指定`,`username`和`password`分别表示`用户名`和`密码`,提供认证机制`需要的`用户名和密码

#### GET
> + GET请求是最常见的请求类型,最常`用于向服务器查询某些信息`,`必要时`,可以将`查询字符串参数追加到URL的末尾,以便提交给服务器`
> + 更常用,更方便,明文发送,相对不是很安全,传输数据有限制,但性能好
> + **注意:** `特殊字符`(eg:`&`等),传参产生的问题可以使用`encodeURIComponent()`进行编码处理,有文字符的返回及传参,`可以将页面保存和设置为` `utf-8`格式即可,或者使用`提交url通用方法`

#### POST
> + POST请求可以包含非常多的数据,我们在使用表单提交的时候,很多就是使用的POST除数方式.
> + 一般用来发送文件,性能没有get方式高,但是安全性好
> + xhr.open("POST","demo.php".true);
> + 而发送POST请求的数据,`不会`跟在`URL`的尾巴上,而是通过`send( )`方法向服务器提交数据,`也就是传输的数据时完全隐藏的`
> + `xhr.send`("name=Lee&age=100");
> + 一般来说,向服务器发送POST请求由于解析机制的原因,需要进行特别的处理,因为POST请求和Web表单提交是不同的,需要使用XHR来模仿表单提交
> + `设置请求头`
> +  + SetRequestHeader( `String header`,`String Value` );
> + 设置HTTP请求中的指定头部`header的值为value`. 
>+ 此方法需在open方法以后调用，一般在post方式中使用。

    //请求头POST方式需要
    xhr.setRuquestHeader('Content-Type','application/x-www-form-urlencoded');

#### `send`(data)
> + 向服务器发出请求,如果采用异步方式,该方式立即返回
> + content可以指定为`null表示不发送数据`,其内容可以说`DOM对象`,输入流或字符串

#### `Abort()`
> + `停止当前http请求`,对应的`XMLHttpRequest对象会复位到未初始化的状态`

### XMLHttpRequest对象属性
**onreadystatechange**
> + `请求状态改变的事件触发器`(readyState变化时`会调用这个属性上注册`的JavaScript函数)

#### readySate

> + 表示XMLHttpRequest对象的状态: 
> + 0 : 未初始化,对象已创建,     ------------------   未调用open;
> + 1 : open方法成功调用,但Send方法未调用;    -------`请求建立,但是没有发送`
> + 2: send方法已经调用,      -------------------    尚未开始接受数据;
> + 3: 正在接受数据,Http响应头信息已经接受,      ---------------   但尚未接收完成;
> + 4 : 完成,即响应数据接收完成

#### responseText | responseXML

> + responseText 服务器响应`(返回)`的文本内容
> + responseXML服务器响应的XML内容对应的DOM对象

### Status
> + 服务器返回的http状态码
> + 200表示**成功**
> + 404表示**未找到**
> + 500表示**服务器内部错误**`等`

+ **通过xhr.getResponseHeader("Content-Type");可以`获取单个响应头信息`**
+ **getAllresponseHeaders( )**;`获取整个响应头信息`

### 文件下载
- 一般的文件下载请先压缩一下,否则可能会是打开,而不是下载
- 要下载的文件得是文件地址,否则都是打开