# url/queryString/assert/crypto

## url

- 拿到 **url** 模块要使用new关键字来执行

```js
// 地址 "https://www.baidu.com/baidu?isource=infinity&iname=baidu&itype=web&tn=90847716_s_hao_pg&ch=13&ie=utf-8&wd=%E6%88%91%E4%B8%8D%E9%94%99"
const url = require("url").URL;

let address = new url("https://www.baidu.com/baidu?isource=infinity&iname=baidu&itype=web&tn=90847716_s_hao_pg&ch=13&ie=utf-8&wd=%E6%88%91%E4%B8%8D%E9%94%99");


console.log(address.searchParams.get("ie"));//utf-8
console.log(address);

//输出
URL {
  href:
   'https://www.baidu.com/baidu?isource=infinity&iname=baidu&itype=web&tn=90847716_s_hao_pg&ch=13&ie=utf-8&wd=%E6%88%91%E4%B8%8D%E9%94%99',
  origin: 'https://www.baidu.com',//序列化的URL origin部分
  protocol: 'https:',//协议
  username: '',//用户名
  password: '',//密码
  host: 'www.baidu.com',//主机
  hostname: 'www.baidu.com',//主机名
  port: '',//端口
  pathname: '/baidu',//包含 URL 的整个路径部分
  search://包含 URL 的整个查询字符串部分,以?的后面部分
   '?isource=infinity&iname=baidu&itype=web&tn=90847716_s_hao_pg&ch=13&ie=utf-8&wd=%E6%88%91%E4%B8%8D%E9%94%99',
  searchParams://表示URL查询参数的URLSearchParams对象。该属性是只读的
   URLSearchParams {//这里是个Map,拿到它要用Map的get方式   该属性是只读的
      'isource' => 'infinity',
      'iname' => 'baidu',
      'itype' => 'web',
      'tn' => '90847716_s_hao_pg',
      'ch' => '13',
      'ie' => 'utf-8',
      'wd' => '我不错' },
      hash: '' //hash值
	}

```

### url.resolve( from, to )

- `url.resolve()` 方法会以一种 Web 浏览器解析超链接的方式把一个目标 URL 解析成相对于一个基础 URL 
- 注意url

```js
const url = require('url');
url.resolve('/one/two/three', 'four');         // '/one/two/four'
url.resolve('http://example.com/', '/one');    // 'http://example.com/one'
url.resolve('http://example.com/one', '/two'); // 'http://example.com/two'
url.resolve("url/local","node"); //url/node
url.resolve("url/*","node");//url/node
url.resolve("url/a","../node");//node
```

## queryString 查询字符串

- 返回一个json

```js
const queryString = require("queryString");
```

### querystring.parse(str[, sep[, eq[, options]]])

- `str` [string](http://nodejs.cn/s/_moz/Data_structures#String_type) 要解析的 URL 查询字符串。
- `sep` [string](http://nodejs.cn/s/_moz/Data_structures#String_type) 用于界定查询字符串中的键值对的子字符串。默认为 `'&'`。
- `eq` [string](http://nodejs.cn/s/_moz/Data_structures#String_type) 用于界定查询字符串中的键与值的子字符串。默认为 `'='`。
- `options` [Object](http://nodejs.cn/s/_moz/Reference/Global_Objects/Object)
  - `decodeURIComponent` [Function](http://nodejs.cn/s/_moz/Reference/Global_Objects/Function) 解码查询字符串的字符时使用的函数。默认为 `querystring.unescape()`。
  - `maxKeys` [number](http://nodejs.cn/s/_moz/Data_structures#Number_type) 指定要解析的键的最大数量。指定为 `0` 则不限制。默认为 `1000`。

该方法会把一个 URL 查询字符串 `str` 解析成一个键值对的集合。

- **注意:** 该方法返回的对象不继承自 JavaScript 的 `Object` 类。 这意味着 `Object` 类的方法如 `obj.toString()`、`obj.hasOwnProperty()` 等没有**被定义且无法使用** 
- 使用   结合url模块使用, 直接返回 **URLSearchParams**

```js
const url = require("url").URL,
    queryString = require("querystring");

let address = new url("https://www.baidu.com/baidu?isource=infinity&iname=baidu&itype=web&tn=90847716_s_hao_pg&ch=13&ie=utf-8&wd=%E6%88%91%E4%B8%8D%E9%94%99");

console.log(queryString.parse(address.search.slice(1)));

//{ 
	 isource: 'infinity',
      iname: 'baidu',
      itype: 'web',
      tn: '90847716_s_hao_pg',
      ch: '13',
      ie: 'utf-8',
      wd: '我不错' 
  }

```

## assert

- 断言模板
- 传一个值进去,如果没达到我的预期值,就会报错

```js
const assert = require("assert");
```

### equal( )方法

- 使用 **==** 符号,不等就报错

```js
assert.equal(1,2);//报错
```

- 注意 : 在有数字参与的情况下,就会都尽量转化成数字

### notEqual( ) 方法

- 和equal相反

```js
assert.notEqual(1,1);//报错
```

### strictEqual( ) 方法

- 内部使用 **===** 
- 使用 [SameValue比较法](http://nodejs.cn/s/25ULs2) 测试 `actual` 参数与 `expected` 参数是否全等 

```js
assert.equal(1,"1");//不报错   会进行类型转换

assert.strictEqual(2,"2");//报错 不会进行类型转换

```

### notStrictEqual( ) 方法

- 和strictEqual( ) 相反

### deepEqual( )

- 判断对象的属性是否一一对应( **相等** )
- 用了Object.create( ) 方法也会报错
- 不使用 **===** 来进行判断

```js
const assert = require('assert');

const obj1 = {
  a: {
    b: 1
  }
};
const obj2 = {
  a: {
    b: 2
  }
};
const obj3 = {
  a: {
    b: 1
  }
};
const obj4 = Object.create(obj1);

assert.deepEqual(obj1, obj1);
// OK

// Values of b are different:
assert.deepEqual(obj1, obj2);
// AssertionError: { a: { b: 1 } } deepEqual { a: { b: 2 } }

assert.deepEqual(obj1, obj3);
// OK

// Prototypes are ignored:
assert.deepEqual(obj1, obj4);
// AssertionError: { a: { b: 1 } } deepEqual {}
```

### notDeepEqual( )

- 和deepEqual( )相反

### 注 : 

- 上面的报错信息可以使用 try catch中的catch来捕捉

## crypto 加密模块

```js
const crypto = require("crypto");

console.log(crypto.getHashes());  //返回加密方式,返回一个数组  

const str = "111223344555";

const obj = crypto.createHash("md5");//实参内写加密方式,加密方式可以通过getHashes方式拿到

obj.update(str);

let password = obj.digest("hex");//十六进制  base64   latin1   hex

console.log(password);//5a592faf95d6820648115c5fa2b1a7f4
```

