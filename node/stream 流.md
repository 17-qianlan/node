# stream 流

## 创建读取流对象

```javascript
const Readable = require("stream").Readable,
    rs = new Readable;//可读流
const write = require("fs").createWriteStream("./3.txt");

rs.push("1","utf-8");
let s = "飞机飞机返回天发发火日放假放假放假";
rs.push(s,"utf-8");
rs.push(null);//在这里不可以再写push了,否则会报错
// console.log(rs.pipe(process.stdout)); 这里打印会把这个数据结构打印出来    直接输出(在app页面内)

rs.pipe(write);
```

