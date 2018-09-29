## 访问ejs文件和使用MySQL请求数据

### router.js

```js
const http = require("http"),
    express = require("express"),
    router = express.Router();
router.get("/",(req,res) =>{
   //下面的路径可以写../views/index.ejs,也可以只写index.ejs  自己会找
   res.render("../views/index.ejs",{name:"<h1>浅蓝</h1>"});
});

//要导出
module.exports = router;
```

### views.ejs

```ejs
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <% include 555.ejs%>//导入其他的ejs文件
    <br>
    <%- name %>  //-表示未转译,可以正常识别html代码
    <%= name %>   //=表示已转译,写什么输出什么
    <br>
    <% for(let i=0;i<10;i++){ %>
        <p>i</p>  //不需要使用%,导入进来的需要使用%
    <% }%>//这些语句要使用%包起来
    <img src="4.jpg" alt="">
</body>
</html>
```

### app.js

```js
const http = require("http"),
    express = require("express"),
    app = express();

//设置模板引擎
app.set("views",__dirname+"/views");

//设置模板引擎是什么

app.set("views engine","ejs");
//访问router.js
app.use("/",require("./router/router.js"));

//加载静态资源  js css  img等之类的东西   + 后面是路径
//在public文件里存入静态文件
app.use(express.static(__dirname+"/public"));

http.createServer(app).listen(2020);
```

### MySQL

- 存在于module文件下的 **js** 文件

```js
const mysql = require("mysql");

module.exports = (sql,cb) => {
    let config = mysql.createConnection({
        //地址
        host : "localhost",
        //本地根目录
        user : "root",
        //密码
        password : "",
        //端口
        port : "3306",
        //数据地址
        database : "qianlan"
    });

    config.connect();
    //"select * form article"
    config.query(sql,(err,data) => {
        cb(err, data);
    });

    config.end();
};
```

- 下面是 **router.js** 文件

```js
const express = require("express"),
    router = express.Router(),
    sql = require("./../module/mysql.js");

router.get("/",(req,res) =>{
    sql("select * from article",(err,data) => {
        res.render("index.ejs",{data:data});
    });
});

module.exports = router;
```

