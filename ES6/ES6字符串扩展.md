## ES6字符串扩展

### includes()、startsWith()、endsWith()
#### 概述
```
传统上，JavaScript 只有indexOf方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6 又提供了三种新方法。
```
#### 特点
- **includes()**：返回布尔值，表示**是否找到**了参数字符串。
- `startsWith()`：返回布尔值，表示参数字符串是否在原字符串的**头部**。
- **endsWith()**：返回布尔值，表示参数字符串是否在原字符串的**尾部**。

    let s = 'Hello world!';
    
    s.startsWith('Hello') // true
    s.endsWith('!') // true
    s.includes('o') // true

这三个方法都支持第二个参数，表示开始搜索的位置。

    let s = 'Hello world!';
    
    s.startsWith('world', 6) // true
    //从开始的第6位后开始找
    s.endsWith('Hello', 5) // true
    //从结束的第5位前开始找
    s.includes('Hello', 6) // false
    //

**上面代码表示，使用第二个参数n时，endsWith的行为与其他两个方法有所不同。它针对前n个字符，而其他两个方法针对从第n个位置直到字符串结束。**  

### repeat

- repeat方法**返回一个新字符串**，表示将**原字符串**重复n次。

    'x'.repeat(3) // "xxx"
    'hello'.repeat(2) // "hellohello"
    'na'.repeat(0) // ""

- 参数如果是小数，会被向下取整。
- 如果repeat的参数是`负数或者Infinity`，会报错。
- 0 到-1 之间的小数，**则等同于 0**，这是因为会先进行取整运算。0 到-1 之间的小数，取整以后等于-0，repeat视同为 0。
- 参数`NaN`等同于 0。
- 如果repeat的参数是字符串，则会先转`换成数字`。

### padStart()、padEnd()
#### 特点
> ES2017 引入了字符串**补全长度**的功能。
> 如果某个字符串`不够`指定长度，会在头部或尾部**补全**。
> **padStart()**用于头部补全，**padEnd()**用于尾部补全。

    'x'.padStart(5, 'ab') // 'ababx'
    'x'.padStart(4, 'ab') // 'abax'
    
    'x'.padEnd(5, 'ab') // 'xabab'
    'x'.padEnd(4, 'ab') // 'xaba'

- 如果原字符串的长度，**等于或大于**指定的最小长度，则返回原字符串。
- 如果用来补全的字符串与原字符串，两者的长度之和超过了指定的最小长度，则会`截去超出位数`的补全字符串。
- 如果省略第二个参数，默认**使用空格补全长度**。

### 模板字符串
#### 区别`(ES5)`
es5的字符串模板输出通常是使用+拼接。

> 这样的缺点显然易见：字符串拼接内容多的时候，过于混乱，易出错。

#### ES6 的模板字符串。

    var name = "风屿",trait = "帅气";
    //es5
    var str = "他叫"+name+",人非常"+trait+",说话又好听";
    
    //es6
    var str2 = `他叫 ${name} ,人非常 ${trait} ,说话又好听`;

> 模板字符串是**增强版的字符串**，用反引号（`）标识。
> 它可以当作普通字符串使用，也可以用来**定义多行**字符串，或者在字符串中嵌入变量。

- 如果在模板字符串中需要使用反引号，则前面要用**反斜杠转义**。
- 如果使用模板字符串表示多行字符串，所有的空格和缩进`都会被保留在`输出之中。
- 模板字符串中**嵌入变量**，需要将变量名**写在${}**之中。
- 大括号内部可以放入任意的 JavaScript 表达式，可以**进行运算，以及引用对象属性**。
- 模板字符串之中还能调用函数。
- 如果大括号中的值不是字符串，将**按照一般**的规则转为字符串。比如，大括号中是一个对象，将默认调用对象的`toString方法`。
- 如果模板字符串中的`变量没有声明`，将报错。

#### 标签模板

> 模板字符串可以紧跟在一个函数名后面，该函数将被调用来处理这个模板字符串。这被称为“**标签模板**”功能。

    alert`123`
    // 等同于
    alert(123)

- 标签模板其实不是模板，而是**函数调用**的一种特殊形式。
- **“标签”指的就是函数**，紧跟在后面的模板字符串就是它的参数。
- 如果模板字符里面有变量，就不是简单的调用了，而是**会将模板字符串**先处理成多个参数，再调用函数。

    let a = 5;
    let b = 10;
    
    tag`Hello ${ a + b } world ${ a * b }`;
    // 等同于
    tag(['Hello ', ' world ', ''], 15, 50);
    
    function tag(stringArr, value1, value2){
		  // ...
	 }

	// 等同于
	function tag(stringArr, ...values){
		  // ...
	}
	//这个例子更好
	let a = 5;
	let b = 10;
	
	function tag(s, v1, v2) {
	  console.log(s[0]);
	  console.log(s[1]);
	  console.log(s[2]);
	  console.log(v1);
	  console.log(v2);
	
	  return "OK";
	}
	
	tag`Hello ${ a + b } world ${ a * b}`;
	// "Hello "
	// " world "
	// ""
	// 15
	// 50
	// "OK"
	
	//这个例子稍微复杂一些
	let total = 30;
	let msg = passthru`The total is ${total} (${total*1.05} with tax)`;
	
	function passthru(literals) {
	  let result = '';
	  let i = 0;
	
	  while (i < literals.length) {
	    result += literals[i++];
	    if (i < arguments.length) {
	      result += arguments[i];
	    }
	  }
	
	  return result;
	}
	msg // "The total is 30 (31.5 with tax)"
	//展示了如何将各个参数按照原来的位置拼合回去。



