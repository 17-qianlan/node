# 文件系统(fs)

```js
const fs = require("fs");
```

## 读   readFile

### 异步的

- fs.readFile( 路径, 指定编码集 , callback);
- - 编码集常用"utf-8"
  - callback   **(err,data) => { }**

```js
fs.readFile("./txt/1.txt","utf-8",((err, data) => {
    console.log(data);//jjssjdvnvhfeijfowmsvnfjs;a   需要指定编码集,否则返回的是
    //<Buffer 6a 6a 73 73 6a 64 76 6e 76 68 66 65 69 6a 66 6f 77 6d 73 76 6e 66 6a 73 3b 61>
}));
```

### 同步的

- fs.readFileSync( 路径,指定编码集)
- - 不需要回调函数

```js
let f = fs.readFileSync("./txt/1.txt","utf-8");
```

## 写   writeFile

### 异步的

- fs.writeFile(路径, 数据,指定字符集, callback);
- - 路径就是要创建的文件的路径
  - 数据是创建文件的内容

```js
let b = "2233390098774467758394";
fs.writeFile("./txt/2.txt",b,(err,data) => {
    console.log(err, data);// err  null      data  undefined
});

```

### 同步的

- **fs.writeFile(路径, 数据,指定字符集);**

```js
fs.writeFileSync("./txt/2.txt",b);//如果遇到相同的路径则会覆盖
```

## 确定路径是否正确

### fs.existsSync( 路径 )

- 返回布尔值   true/false    
- 异步的已被废弃

## 创建文件夹

### 异步的

- **fs.mkdir( 路径,mode(权限),callback)**

```js
fs.mkdir("./txt/a",(err,data) => {
    console.log(err, data); //null undefined
});
```

### 同步的

- fs.mkdirSync( 路径,mode(权限 );

```js
fs.mkdirSync("./txt/b");
```

## 读文件夹

### 异步的

- fs.readdir(路径,(err,data) => { })
- - data返回一个数组,该路径下对应的文件名
  - 其中 `data` 是目录中不包括 `'.'` 和 `'..'` 的文件名的数组 

```js
fs.readdir("./txt",(err,data) => {
    console.log(err, data);//null [ '1.txt', '2.txt', 'a', 'b' ]
});

```

### 同步的

- fs.readdirSync( 路径)
- 返回的包含文件夹,当前文件夹下的所有文件夹和文件

```js
console.log(fs.readdirSync("./txt"));//[ '1.txt', '2.txt', 'a', 'b' ]
```

## 判断是否是文件夹

- fs.statSync( 路径 ) 
- 这个用同步就可以了

```js
console.log(fs.statSync("./txt/a"));//如果没问题,返回一个json
// Stats {
      dev: 1119068372,
      mode: 16822,
      nlink: 1,
      uid: 0,
      gid: 0,
      rdev: 0,
      blksize: undefined,
      ino: 6192449487921299,
      size: 0,
      blocks: undefined,
      atimeMs: 1535002500938.025,
      mtimeMs: 1535002482813.404,
      ctimeMs: 1535002482813.404,
      birthtimeMs: 1535002482813.404,
      atime: 2018-08-23T05:35:00.938Z,
      mtime: 2018-08-23T05:34:42.813Z,
      ctime: 2018-08-23T05:34:42.813Z,
      birthtime: 2018-08-23T05:34:42.813Z 
  }
```

```js
const stat = fs.statSync( 路径 );
```

- stat.isDirectory( );
- - 返回布尔值  true/false

```js
console.log(stat.isDirectory());//true
```

## 流读取

```js
const read = require("fs").createReadStream(路径);
//设置字符编码
read.setEncoding("utf8");
```

### 改变状态(从静止变为流动)

```js
read.resume();//使用data时间则不需要此行
```

### 创建流读取事件

```js
//边读取边触发,会分组读取,性能较好
read.on("data",data => {
    console.log(data);
});

//只有读取完毕后触发
read.on("end",()=>{
    console.log("读取结束");
});
```

### 写入流

```javascript
const write = require("fs").createWriteStream( 路径 );
read.pipe(write);

```

