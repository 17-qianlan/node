# koa

## ctx和next

- ctx下的所有参数

```json
{
  request: {
    method: 'GET',
    url: '/',
    header: {
      host: 'localhost:3003',
      connection: 'keep-alive',
      'cache-control': 'max-age=0',
      'upgrade-insecure-requests': '1',
      'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0 .3440 .106 Safari / 537.36 OPR / 55.0 .2994 .44(Edition Baidu)',
      accept: 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8',
      'accept-encoding': 'gzip, deflate, br',
      'accept-language': 'zh-CN,zh;q=0.9'
    }
  },
  response: {
    status: 404,
    message: 'Not Found',
    header: {}
  },
  app: {
    subdomainOffset: 2,
    proxy: false,
    env: 'development'
  },
    //这几个是原生node的方法,可以直接ctx.req调用
  originalUrl: '/',
  req: '<original node req>',
  res: '<original node res>',
  socket: '<original node socket>'
}
```

- next

## Ajax请求

```js
//app.js

const Koa = require("koa"),
    cors = require("@koa/cors");

const koa = new Koa;

let num = 88;

koa.use(cors());//解决跨域问题

koa.use(async (ctx,next) => {
    let obj = {
        a : 3,
        b : 5,
        d : num,
        c : "我是一个小小的石头"
    };
    ctx.body = `<h1>${JSON.stringify(obj)}</h1>`;
});

koa.listen(3003);
```

- html

```html
<script>
    $.ajax({
        url : "http://localhost:3003",
        method : "GET",
        success(msg) {
            $("body").html(msg);
            // console.log(msg);
        }
    })
</script>
```

- package.json

```json
{
  "name": "koa",
  "version": "1.0.0",
  "dependencies": {
    "koa": "latest",
    "@koa/cors": "latest"
  }
}

```

## 接收ajax发过来的数据

#### 所需的中间件

```js
koa  //这个是依赖
   koa-static    --- 加载静态文件
   koa-router    --- 路由
   koa-body      --- 接收ajax数据
   @koa/cors     --- 跨域
   
   
   
//package.json
{
  "name": "koaRouter",
  "version": "1.0.0",
  "dependencies": {
    "koa": "latest",
    "koa-router": "latest",
    "@koa/cors": "latest",
    "koa-body": "latest",
    "koa-static": "latest"
  }
}
```



- ajax.html

```js
$li = $("ul li");
(function(){
    $li.each(index => {
        $li.eq(index).click(() => {
            $.ajax({
                url : `http://localhost:3000/${$li.eq(index).text()}`,
                method : "post",
                data : {
                    b : 5,
                    d : "我好没毅力555"
                },
                success(msg) {
                    $(".text").html(msg);
                }
            })
        }).css("background" ,"#98d4ff");
    });
})()
```

- app.js

```js
const Koa = require("koa"),
    cors = require("@koa/cors"),
    serve = require("koa-static"),
    router = require("./router/ruoter");


const koa = new Koa;
let o = {
    a : 2,
    c : "我是一个"
};

//读取静态文件,否则这个案例就不成功
koa.use(serve(__dirname + './public/ajax.html'));

koa.use(cors());

koa.use(router.routes());//使路由挂载到koa上


koa.listen(3000);
```

- router.js

```js
const Router = require("koa-router"),
    koaBody = require("koa-body");

const router = new Router;

//这里边的koaBody()
router.post("/index",koaBody(),async (ctx,next) => {
    let data = ctx.request.body;
    data["index"] = "index";
    ctx.body = JSON.stringify(data);
});
router.post("/admin",koaBody(),async (ctx,next) => {
    let data = ctx.request.body;
    data["admin"] = "admin";
    ctx.body = JSON.stringify(data);
});
router.post("/user",koaBody(),async (ctx,next) => {
    let data = ctx.request.body;
    data["user"] = "user";
    ctx.body = JSON.stringify(data);
});
router.post("/home",koaBody(),async (ctx,next) => {
    let data = ctx.request.body;
    data["home"] = "home";
    ctx.body = JSON.stringify(data);
});



module.exports = router;
```





## 生成一些文件直接读取

```js
const Koa = require("koa"),
    Router = require("koa-router"),
    fs = require("fs");

const koa = new Koa,
    router = new Router;



let data = fs.readFileSync("./public/test.html","utf-8");
koa.use(async ctx => {
    ctx.body = data;//返回到前端页面可以直接被读取并且正确的识别出来(包括js)
});




koa.listen(3000);
```



## 一些参数

### 改变status

```js
ctx.status = 202;//修改status为202
```

## koa中间件

### koa-router 路由

#### 挂载到koa上

- 必须有的,不然router.get/router.post等方式不会生效

```js
koa.use(router.routes()).use(router.allowedMethods());
```



- app.js

```js
const Koa = require("koa"),
    Router = require("koa-router"),
    cors = require("@koa/cors");


const koa = new Koa,
    router = new Router;
let o = {
    a : 2,
    c : "我是一个"
};
koa.use(cors());//解决跨域
router.get("/index", async (ctx,next) => {
    ctx.body = JSON.stringify(o);
});
router.post("/index", async (ctx,next) => {
    ctx.body = JSON.stringify({
        b : 5,
        d : "我好没毅力"
    });
});

koa.use(router.routes()).use(router.allowedMethods());
koa.listen(3000);
```

- ajax.html

```html
<script>
    $.ajax({
        url : "http://localhost:3000/index",
        method : "get",//这里会根据请求方式的不同而请求到对应的数据
        success(msg) {
            $("body").html(`<h1>${msg}</h1>`);
        }
    })
</script>
```

#### router.post/router.get

- 只接收post或者get的请求方式

