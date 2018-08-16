##六大数据类型之Object
>1. ECMAScript中的对象其实就是一组数据和功能的集合
>1. 对象可以通过new操作符后跟要创建的类型的名称来创建
>1. 而创建Object类型的实例并为其添加属性和（或）方法，就可以创建自定义对象

    var obj = new Object（）；
    obj.name = "wo";//创建（或赋值）自定义对象
 
**在括号内不写参数的时候，也可以这样创建，但不推荐**

    var obj = new Object；
**Object的每个实例都具有下列属性和方法**
> + Constructor：保存着用于创建当前对象的函数。对于前面的例子而言，构造函数就是Object（）；
> + hasOwnProperty（propertyName）：用于检查给特定的属性再当前对象实例中（而不是在实例的原型中）是否存在；参数（properName）必须是字符串的形式    obj.hasOwnProperty("name");
> + isPrototypeOf(object) ：用于检测传入的对象是否是另一个对象的原型
> +  toLocaleString（）：返回对象的字符串表示，该字符串与执行环境的地区对应。
> + toString（）：返回对象的字符串表示；
> valueOf（ ）：返回对象的字符串、数值或布尔值表示。通常与toString（）相同

**对象有**
> Object节点对象、对象、数组[ ]、{ json }、null

- JSON
> json的本质就是一个字符串
> 都要用双引号引起来,如果出现了语法的错误,请用`\`转义


 **创建对象**
 >1. 第一种方式：通过new操作符
 >2. 第二种方式：使用对象字面量表示法
 > + 对象字面量是对象定义的一种简写形式，目的在于简化创建包含大量属性的对象过程`json`

    var person = {
	    name ：“hello”，
	    age ：29//此处如果有逗号，则在IE7以下和Opera中导致错误
    }；
    //还可以这样
    var person = {
	    “name” ：“hello”，
	    “age” ： 23，
	    5 ：true//这里的5会被自动转换成一个字符串
    }
    //与new Object（）相同
    var person = {}；
    person.name = “hello”；
    person.age = 29；
>对象字面量方法一般只在考虑对象属性的可读时使用

**访问方法**
>一般有两种访问方式：`方括号和 .`
>.访问中属性名不可以有空格；而方括号是可以由 

####null
>Null类型是第二个只有一个值得数据类型
>null值表示一个空对象指针

    alert(typeof null);//object
>如果定义的变量准备在将来用于保存对象,那么最好将该变量初始化为null而不是其他值,这样就可以只要直接检查null值就可以知道相应的变量是否已经保存了一个对象的应用

    if(car != null ) {
	    //执行某些操作
    }
>实际上,undefined值是派生自null值的,因此ECMA-262规定他们的相等新测试返回true

    alert(null == undefined);//true


