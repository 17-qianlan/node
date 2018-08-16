

## 与JS的区别及一些简单操作

### 注册了一个事件

- 查空房.on('有空房')
- 查空房.on('没空房')function
- 查空房.on(err)

### 浏览器中的js 和node有什么区别

- window      node: 全局对象global
- 作用域 :  1个文件是一个作用域
- 调试   console.log() 只有 没有alert

### 模块 -----在 node 中一个文件可以看作是一个模块

#### 引用 require('./1')

- 后缀**.js**可以省略
-  如果引入模块  是  模块的名字 代表是核心模块 : 
- 是安装好node就有的一些模块
- node_modules这个文件夹下面
- 引入的路径 如果是自己定义的模块最好是 ./ 或 ../  来引用
- 模块的加载机制: 文件名 > 文件名.js > 文件名.json > 文件名文件名.node
- 模块之间怎么互相使用

### node目录的配置

- 配置文件 : package.json

- - dependencies :  当前项目所使用到的依赖模块

- 安装方式: npm install  自动读取package.json自动安装

  - router目录 用来存放路由文件
  - views目录  用来存放html模板文件module 目录  自己写的一些模块

```js
{
  "name": "nodevip",
  "version": "0.0.1",
  "dependencies": {
    "express":"latest"
  }
}
```

### 导出

- 多次书写会覆盖

```js
module.exports = {}
```

- 用这种方法不会覆盖

```js
//可以通过点操作的方式进行点操作
module.exports.fn = {};
module.exports.num = {};
```

- 快捷的操作

```js
global.exports = module.exports;
==>
可以把global省略
```

