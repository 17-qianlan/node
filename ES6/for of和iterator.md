## for of 和 iterator

### 使用for of( 概念 )

- 一个数据结构只要部署了`Symbol.iterator`属性，就被视为具有 iterator 接口，就可以用`for...of`循环遍历它的成员。也就是说，`for...of`循环内部调用的是数据结构的`Symbol.iterator`方法 
- 也就是说要使用for of那么一定支持`Symbol.iterator`
- `for...of`循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如`arguments`对象、DOM NodeList 对象）、后文的 Generator 对象，以及字符串 

#### 数组使用 for of

- 证明数组有Symbol.iterator属性,也就是默认部署

```js
const arr = ['red', 'green', 'blue'];

for(let v of arr) {
  console.log(v); // red green blue
}

const obj = {};
obj[Symbol.iterator] = arr[Symbol.iterator].bind(arr);

for(let v of obj) {
  console.log(v); // red green blue
}
```

- **计算生成的数据结构**

> 有些数据结构是在现有数据结构的基础上，计算生成的。比如，ES6 的数组、Set、Map 都部署了以下三个方法，调用后都返回遍历器对象。
>
> - `entries()` 返回一个遍历器对象，用来遍历`[键名, 键值]`组成的数组。对于数组，键名就是索引值；对于 Set，键名与键值相同。Map 结构的 Iterator 接口，默认就是调用`entries`方法。
> - `keys()` 返回一个遍历器对象，用来遍历所有的键名。
> - `values()` 返回一个遍历器对象，用来遍历所有的键值

- 类似数组的对象
- - 不是所有类似数组的对象都具有 Iterator 接口，一个简便的解决方法，就是使用**`Array.from`**方法将其转为数组 

```js
let arrayLike = { length: 2, 0: 'a', 1: 'b' };

// 报错
for (let x of arrayLike) {
  console.log(x);
}

// 正确
for (let x of Array.from(arrayLike)) {
  console.log(x);
}
```

- - 下面是`for...of`循环用于字符串、DOM NodeList 对象、`arguments`对象的例子 

```js
// 字符串
let str = "hello";

for (let s of str) {
  console.log(s); // h e l l o
}

// DOM NodeList对象
let paras = document.querySelectorAll("p");

for (let p of paras) {
  p.classList.add("test");
}

// arguments对象
function printArgs() {
  for (let x of arguments) {
    console.log(x);
  }
}
printArgs('a', 'b');
// 'a'
// 'b'
```

- 可以正确识别32 位 UTF-16 字符 
- 对象
  -  对于普通的对象，`for...of`结构不能直接使用，会报错，必须部署了 Iterator 接口后才能使用
  - 但是，这样情况下，`for...in`循环依然可以用来遍历键名 

```js
let es6 = {
  edition: 6,
  committee: "TC39",
  standard: "ECMA-262"
};

for (let e in es6) {
  console.log(e);
}
// edition
// committee
// standard

for (let e of es6) {
  console.log(e);
}
// TypeError: es6[Symbol.iterator] is not a function
```

- **一种解决方法是**，使用`Object.keys`方法将对象的键名生成一个数组，然后遍历这个数组 

```js
for (var key of Object.keys(someObject)) {
  console.log(key + ': ' + someObject[key]);
}
```

- 另一个方法是使用 Generator 函数将对象重新包装一下 

```js
function* entries(obj) {
  for (let key of Object.keys(obj)) {
    yield [key, obj[key]];
  }
}

for (let [key, value] of entries(obj)) {
  console.log(key, '->', value);
}
// a -> 1
// b -> 2
// c -> 3
```



- 补上了for in中的缺点

### iterator

#### 不过如果没有iterator可以部署一下

```js
class RangeIterator {
    constructor(start, stop) {
        this.start = start;
        this.stop = stop;
        this.boo = true;
    }

    [Symbol.iterator]() {return this;}//调用这个接口，就会返回一个遍历器对象
    //凡是部署了Symbol.iterator属性的数据结构，就称为部署了遍历器接口

    next() {//这个方法是Symbol.iterator这个自动调用
        let start = this.start;
        if ( start < this.stop ) {
            this.start++;
            return {
                done: false,
                value: start
            }
        }
        this.boo = false;
        return {
            done: true,//一定要是true,不让for of会爆炸
            value: undefined
        }
    }
}
function rage (start,stop){
    return new RangeIterator(start,stop);
}
for( let value of rage(0,3) ){
    console.log(value);
}
```

#### Symbol.iterator指针

```js
function Obj(value) {
    this.value = value;
    this.next = null;
}

Obj.prototype[Symbol.iterator] = function() {
    var iterator = { next: next };
    var current = this;
    console.log( current );//要for of执行才会打印出one,否则是这个this指向的原型
    function next() {
        if (current) {
            var value = current.value;
            current = current.next;//调用two,执行到three,最后一次没有了就是null
            console.log(current);
            return { done: false, value: value };
        } else {
            return { done: true };
        }
    }
    return iterator;
};
var one = new Obj(1);
var two = new Obj(2);
var three = new Obj(3);
var four = new Obj(4);
one.next = two;//此处的next相当于json的键名
two.next = three;
three.next = four;
console.log(one);//最早执行
//console.log(two);
for (var k of one){//只有这个才能调用Obj.prototype[Symbol.iterator]这个函数
    console.log(k); // 1, 2, 3
}
```

#### 使用场合

> - 扩展运算符**(...)**
> - yield*
> - 解构赋值
> - for...of
> - Array.from()
> - Map(), Set(), WeakMap(), WeakSet()（比如`new Map([['a',1],['b',2]])`）
> - Promise.all()
> - Promise.race()

#### 遍历器对象的 return()，throw()

- 遍历器对象除了具有`next`方法，还可以具有`return`方法和`throw`方法 
- `return`方法和`throw`方法是否部署是可选的 (next必须)  ------------------ 自己部署

##### return的使用

- `return`方法的使用场合是，如果`for...of`循环提前退出（通常是因为出错，或者有`break`语句 )
- 如果一个对象在完成遍历前，需要清理或释放资源，就可以部署`return`方法 

```js
function readLinesSync(file) {
  return {
    [Symbol.iterator]() {
      return {
        next() {
          return { done: false };
        },
        return() {
          file.close();
          return { done: true };
        }
      };
    },
  };
}
```