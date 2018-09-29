# http

## 创建一个简单的http服务器

```javascript
const http = require("http");

http.createServer((req,res) => {
    res.writeHead(200,{"Content-Type" : "text/html; charset=utf-8"});//设置请求头(响应头)
    res.write("hhhhhhh还好还好或");//这里可以传HTML代码
    res.end();//打断上下文,必须打断,否则浏览器会一直在转圈,因为一直未加载完成
    //这里下面还可以接着写,但是也已经没意义了     不是return
}).listen(3001);
//res.end();这里可以返回数据,也是string和buffer(二进制),拼接到write的后面

```

-  res.end()
- - 只可以一次,多写以第一次为准

## 阻止favicon.ico

```javascript
const http = require("http"),
    url = require("url");
http.createServer((req,res) => {
    if(url.parse(req.url).pathname ==="/favicon.ico" )return;//在这里阻止的
    //这个也可以,不需要url模块
    //if( req.url ==="/favicon.ico" )return;
    res.writeHead(200,{"Content-Type" : "text/html; charset=utf-8"});
    res.write(toString());
    res.end();//打断上下文
    console.log("2");
}).listen(3211);
```

## 一个Ajax请求

- node.js

```js
const http = require("http");
let server = http.createServer((req,res) => {
    res.setHeader("Access-Control-Allow-Origin", "*");//这个一般把它写在到最前面吧
    res.writeHead(200,{"Content-Type" : "text/html; charset=utf-8"});//这个和跨域顺序不可以颠倒
    if( req.url === "/favicon.ico" )return;
     let num = 100;
     let obj = {
        a : "我是一个",
        b : 2,
        c : num
     };
    res.write(`<h1>${JSON.stringify(obj)}<h1>`);
    res.end();
});
server.listen(3322);
```

- HTML(ajax)

```js
<script>
    $.ajax({
        url: "http://localhost:3322",
        method: "GET",
        success(msg){
            $("body").html(msg);//输出到DOM
        }
	})
</script>
```

