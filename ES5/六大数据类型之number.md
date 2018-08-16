##六大数据类型之`number`
#####number `(数字)`
>`e`代表次方   
>3.12e6 === 3.12×10^6
>3.11e9 === 3.11×10^9

如

    var e = 3.12e6;
    console.log(e);//3120000

**isNaN( )**
>用来检测是否是NaN,`是返回true`,不是返回false

**数值范围**
>由于内存的限制,ECMAScript并不能保存所有的数值,ECMAScript能够表示的最小数值保存在Number.MIN_VALUE中--在大多数中,这个值是5e-324
>如果超过这个范围,则显示`(-infinity,infinity)`

**进制**
>写number时可以写十进制(默认)、八进制、十六进制
>八进制的第一位必须写`0`,在严格模式下会报错
>十六进制的第一位必须写`0x`
>不管是八进制还是十六进制,最后都会被浏览器解析为十进制

如:

    var _8 = 070;
    console.log(_8);//56
    var _16 = 0x04f;
    console.log(_16);//79
#####数值转换
**有三个函数可以转化为数值**:`**Number()  parseInt()   parseFloat()**`
**Number( )**函数的转换规则:
> 1. 如果是Boolean值,true和false分别转化为1和0;
> 1. 如果是数字值,只是简单地插入和返回
>1.  如果是null值,返回0;
>1.  如果是undefined,返回NaN
> 1. 如果是对象,则调用对象的valueOf( )方法然后根据前面的规则转换返回的值。如果是转换的结果是NaN，则调用对象的toString（）方法，然后再次依照前面的规则转换返回的字符串值。
> 1. 如果是字符串,遵循下列规则
> + 如果是字符串中只包含数字(包裹前面带正号或负号的情况),则将其转化为十进制数值,即"1"会变成1,"123"会变成123,而"011"会变成11(最前面的零被忽略)
> + 如果字符串中包含有小的浮点格式,如"1.1",则将其转换为对应的浮点数值(同样)也会忽略最前面的零)
> + 如果字符串中包含有效的十六进制格式,例如:"oxf",则将其转换为相同大小的十进制整数值
> + 如果字符串是空的(不包含任何字符),则将其转换为0;
> + 如果字符串中包含除上述之外的字符,则将其转换为NaN;


如:

    var num1 = Number("hello world");//NaN;
    var num2 = Number("");//0
    var num3 = Number ("000011");//11
    var num4 = Number(true);//1 
>但是Number( )函数在转换字符串是比较复杂而且不够合理,在处理函数的时候更常用的是`parseInt( )`

**parseInt( )**
>parseInt( )函数在转换字符串时,更多的是看其是否符合数值模式,他会忽略字符串前面的空格,指到找到第一个非空字符
>如果第一个字符不是数字字符或者负号,parseInt( )就会返回NaN
>如果第一个是数字字符,则继续解析第二个字符,知道解析完或遇到了一个非数字字符
>小数不是有效字符

    var num1 = parseInt("1234blue");//1234
    var num2 = parseInt("");//NaN
    var num3 = parseInt("0xA");//10(返回的十进制)
    var num4 = parseInt(22.5);//22
    var num5 = parseInt("070");//56(返回的八进制)
    var num6 = parseInt("70");//70(十进制)
    var num7 = parseInt("oxf");//15(返回的十六进制)
 >ES3可以正常解析上面的,但是ES5就不能正常解析八进制的而吧前面的0忽略,进而把他当做十进制的来解析
 
 

    var num = parseInt("070");//ES5 70;ES3 56;
    //ES5认为0是十进制
   
   >为了消除这种浏览器的不正确识别,可以指定基数
   
   如:
   

    var num = parseInt("oxAF" , 16);//175
    ox可省
    var num1 = parseInt("AF",16);//175
    var num2 = parseInt("AF");//NaN
 **parseFloat( )**
 >1. 与parseInt( )相类似，parseFloat（）也是从第一个字符（位置0）开始解析每个字符的，而且一直解析到末尾，或者解析遇到一个无效的浮点数字字符为止（第一个小数点有效）
 >2. parseFloat（）可以正确识别八进制，但是十六进制会被解析成0（永远是0）
 >3. 没有和parseInt( )一样可以指定基数
 
 

    var num1 = parseFloat("1234blue");//1234
    console.log(parseFloat("  12b1"))//12(没空格)
    var num2 = parseFloat("0xA");//0
    var num3 = parseFloat("22.5");//22.5
    var num4 = parseFloat("22.34.5");//22.34
    var num5 = parseFloat("0908.5");//908.5
    var num6 = parseFloat("3.123e7");//32150000