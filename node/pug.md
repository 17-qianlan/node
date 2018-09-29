# pug

## 使用pug

- 一定要在生产环境中安装pug模块
- 然后使用koa-views来渲染
- 在使用时pug一定要转译

```js
const views = require("koa-views"),
      {join} = require("path");
koa.use(views(join(__dirname ,"views")),{extension :"pug"});
```

## 在koa中使用pug

### koa-router

```js
const Router = require("koa-router");

const router = new Router;

router.get("/", async ctx => {
    //渲染文件一定要用await,因为渲染文件是异步的
    await ctx.render("111.pug",{b : false});
    //ctx.render使用说明    ctx.render(a,b)
    1. a 是要使用的渲染文件,可以省略后缀名
    2. b 是传过去给pug使用的参数  必须是一个json   对应应该使用的结构赋值接收的
});

module.exports = router;
```

##### 一个使用示例               

- app.js

```js
const Koa = require("koa"),
    pug = require("pug"),
    views = require("koa-views"),
    router = require("./router/routers"),
    {join} = require("path"),
    static = require("koa-static");

const koa = new Koa;

koa.use(views(join(__dirname ,"views")),{extension :"pug"});

koa.use(static(join(__dirname,"public")));

koa.use(router.routes()).use(router.allowedMethods());

koa.listen(3000);
```

- router.js

```js
const Router = require("koa-router");

const router = new Router;

router.get("/", async ctx => {
    await ctx.render("111.pug",{b : false});
});

module.exports = router;
```

- 111.pug

```pug
extends ./222.pug

append js
    script(src="/js/pug.js")
```

- 222.pug

```jade
doctype html
html
    head
        title 2222
        meta(charset="utf-8")
    body
        .ccc   5555555555555555555555555555555555555555555555555555555555555555555555555
        block js
            script(src="/js/index.js")
```

- 文件目录

```text
app.js

public  js     index.js   pug.js

views   111.pug  222.pug

router  router.js
```



