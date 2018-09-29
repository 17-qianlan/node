## node的队列

### macrotasks和microtasks的划分

- 首先这两个有优先级之分,最开始执行macro-task   然后再执行 micro-task
- 都是依次执行一个,以此循环
- 都是异步,其实异步指的的是回调函数,而不是函数本身

#### macro - task:

- script(全部代码)
- setTimeout
- setInterval
- - 上面这两个定时器是 **谁先注册谁先执行**
- setImmediate
- requestAnimationFrame
- I/O
- UI rendering

#### micro - task:

- process.nextTick
- Promises
- - 这里指的是Promises的than方法
- Object.observe
- MutationObserver

> **任务队列中，在每一次事件循环中，macrotask只会提取一个执行，而microtask会一直提取，直到microsoft队列为空为止**



### 一个相对简单的例子

```js
setTimeout(() => console.log("1"));

process.nextTick(() => console.log("2"));

setImmediate(() => console.log("3"));

console.log("4");

4 2 1 3
```



###  执行例子

```js
setTimeout(() => console.log("1"),0);

setImmediate(() => {
    console.log("2");
    process.nextTick(() => console.log("3"));
    console.log("4");
});

//这个Promise方法被当成了js代码,但是then方法是异步的,要等res()执行后才会输出6,也就是等process.nextTick执行完后才会执行
new Promise((res) => {
    res();//要执行这个参数才会调用then方法
    console.log("5");
}).then(() => {
    console.log("6");
});

process.nextTick( () => console.log("7") );


console.log("8");



// 输出  5 8 7 6 1 2 4 3
```

### 注意

- 不同的浏览器对 **promise** 的表现不一样   

**[链 接 1](https://www.cnblogs.com/QH-Jimmy/p/6493389.html)**

[链 接 2](https://www.cnblogs.com/tugenhua0707/p/7675185.html)