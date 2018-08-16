## ES6面向对象

### 以前的写法

```js
<script>
    function People(name){
        this.name=name;     
    }
    
    People.prototype.sayName=function(){
        console.log(this.name);
    }
    
    var p=new People('zl');
    p.sayName();  //zl
</script>
```

### ES6的写法

- 需要使用class类
- 自动调用construct
- 共享的方法下载construct的下面

```js
<script>
    class People{   //class开头写,
        constructor (name){     //当使用new来创建对象，会自动调用这个函数
            this.name=name
        }
        sayName(){  //共享的方法（方法会挂载到原型身上）
            console.log(this.name)  
        }
    }
    
    var p=new People('zl');
    p.sayName();  //zl
</script>
```

### ES6的继承

- 使用Extends

- ```js
  class Cat extends Animal{};
  // 定义一个Cat类，该类通过extends关键字，继承了Animal类的所有属性和方法。
  class Cat extends Animal{
      constructor(x,y,name){
          super(x,y);
          this.name=name
      }
  }
  ```

- 必须调用super()方法;

### 静态方法 static

- 如果在一个方法前，加上static关键字，就表示该方法不会被实例继承
- 而是直接通过类来调用，这就称为“静态方法” 

```js
class People {
    static print(){
        console.log(100)
    }
}

var p = new People();

p.print()//报错


People.print();//这样调用
```

- 父级的静态方法可以被子级继承
- 继承后子级可以调用父级的方法
- ES6规定class只有静态方法没有静态属性 

参考链接 https://www.jianshu.com/p/b8908fc3f361