# node核心( event/path)

## event

### newListener事件

新增于: v0.1.26

- `eventName` [any](http://nodejs.cn/s/_moz/Data_structures#Data_types) 要监听的事件的名称
- `listener` [Function](http://nodejs.cn/s/_moz/Reference/Global_Objects/Function) 事件的句柄函数

`EventEmitter` 实例会在一个监听器被添加到其内部监听器数组之前触发自身的 `'newListener'` 事件。

注册了 `'newListener'` 事件的监听器会传入事件名与被添加的监听器的引用

- 这个事件在注册后,只要有自定义的事件就会触发,也就是在绑定的过程中是没有输出**(触发)**

```js
const EventEmitter = require("events"),
    myEmitter = new EventEmitter;


myEmitter.on("newListener",()=>{
    console.log("绑定了一个newListener事件");//打印两次
});


myEmitter.on("ql",()=>{});
myEmitter.on("ql",()=>{});
```

- **removeListener**事件和**newListener**一样的

### on事件

新增于: v0.1.101

- `eventName` [any](http://nodejs.cn/s/_moz/Data_structures#Data_types) 事件名
- `listener` [Function](http://nodejs.cn/s/_moz/Reference/Global_Objects/Function) 回调函数

添加 `listener` 函数到名为 `eventName` 的事件的监听器数组的末尾。 不会检查 `listener` 是否已被添加。 多次调用并传入相同的 `eventName` 和 `listener` 会导致 `listener` 被添加与调用多次。

- 简单的说就是绑定事件

```js
const EventEmitter = require("events"),
    myEmitter = new EventEmitter;
//自定义事件
myEmitter.on("ql/事件名",()=>{});
```

### off 关闭/解绑事件

新增于: v10.0.0

- `eventName` [string](http://nodejs.cn/s/_moz/Data_structures#String_type) | [symbol](http://nodejs.cn/s/_moz/Data_structures#Symbol_type)
- `listener` [Function](http://nodejs.cn/s/_moz/Reference/Global_Objects/Function)
- Returns: [EventEmitter](http://nodejs.cn/api/events.html#events_class_eventemitter)

### once 绑定一次性事件

新增于: v0.3.0

- `eventName` [any](http://nodejs.cn/s/_moz/Data_structures#Data_types) 事件名
- `listener` [Function](http://nodejs.cn/s/_moz/Reference/Global_Objects/Function) 回调函数

添加一个单次 `listener` 函数到名为 `eventName` 的事件。 下次触发 `eventName` 事件时，监听器会被移除，然后调用。

### emit  执行事件

- 按监听器的注册顺序，同步地调用每个注册到名为 `eventName` 事件的监听器，并传入提供的参数。
- 如果事件有监听器，则返回 `true` ，否则返回 `false`。

### getMaxListeners 查看绑定了多少个监听事件

- 返回 `EventEmitter` 当前的最大监听器限制值，该值可以通过 [`emitter.setMaxListeners(n)`](http://nodejs.cn/s/VPJci1) 设置或默认为 [`EventEmitter.defaultMaxListeners`](http://nodejs.cn/s/LwxMek) 

### setMaxListener( n )  设置

- 默认情况下，如果为特定事件添加了超过 `10` 个监听器，则 `EventEmitter` 会打印一个警告。 此限制有助于寻找内存泄露。 但是，并不是所有的事件都要被限为 `10` 个。 `emitter.setMaxListeners()` 方法允许修改指定的 `EventEmitter` 实例的限制。 值设为 `Infinity`（或 `0`）表明不限制监听器的数量。
- 返回一个 `EventEmitter` 引用，可以链式调用

### listeners   返回所有的事件函数

- 不会受setMaxListeners( )的影响,该返回多少就返回多少
- 返回名为 `eventName` 的事件的监听器数组的副本 

## path

```js
const path = require("path");
```

- --dirname    返回当前路径,不包含当前文件
- --fileName   返回当前路径,包含当前文件

### path.join( );

新增于: v0.1.16

- `...paths` [string](http://nodejs.cn/s/_moz/Data_structures#String_type) 一个路径片段的序列
- 返回: [string](http://nodejs.cn/s/_moz/Data_structures#String_type)

`path.join()` 方法使用平台特定的分隔符把全部给定的 `path` 片段连接到一起，并规范化生成的路径。

长度为零的 `path` 片段会被忽略。 如果连接后的路径字符串是一个长度为零的字符串，则返回 `'.'`，表示当前工作目录。

例子：

```js
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..');
// 返回: '/foo/bar/baz/asdf'

path.join('foo', {}, 'bar');
// 抛出 'TypeError: Path must be a string. Received {}'
```

如果任一路径片段不是一个字符串，则抛出 [`TypeError`](http://nodejs.cn/s/Z7Lqyj)。

- 这个不返回绝对路径

### path.resolve( )

- 返回的是绝对路径

  新增于: v0.3.4

- `...paths` [string](http://nodejs.cn/s/_moz/Data_structures#String_type) 一个路径或路径片段的序列

- 返回: [string](http://nodejs.cn/s/_moz/Data_structures#String_type)

  `path.resolve()` 方法会把一个路径或路径片段的序列解析为一个绝对路径。

  给定的路径的序列是从右往左被处理的，后面每个 `path` 被依次解析，直到构造完成一个绝对路径。 例如，给定的路径片段的序列为：`/foo`、`/bar`、`baz`，则调用 `path.resolve('/foo', '/bar', 'baz')` 会返回 `/bar/baz`。

  如果处理完全部给定的 `path` 片段后还未生成一个绝对路径，则当前工作目录会被用上。

  生成的路径是规范化后的，且末尾的斜杠会被删除，除非路径被解析为根目录。

  长度为零的 `path` 片段会被忽略。

  如果没有传入 `path` 片段，则 `path.resolve()` 会返回当前工作目录的绝对路径。

  例子：

  ```js
  path.resolve('/foo/bar', './baz');
  // 返回: '/foo/bar/baz'
  
  path.resolve('/foo/bar', '/tmp/file/');
  // 返回: '/tmp/file'
  
  path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif');
  // 如果当前工作目录为 /home/myself/node，
  // 则返回 '/home/myself/node/wwwroot/static_files/gif/image.gif'
  ```

  如果任何参数不是一个字符串，则抛出 [`TypeError`](http://nodejs.cn/s/Z7Lqyj)。

  - 这个返回的是绝对路径

  ### path.parse( );

  - 解析路径,返回一个json对象
  - 有当前的文件的路径,文件名和类型

  ```js
  console.log(path.parse(__filename));
  
  { 
      root: 'C:\\',
      dir: 'C:\\Users\\qlhdt\\Desktop\\node\\path',
      base: 'app.js',
      ext: '.js',
      name: 'app' 
  }
  ```

  

