##六大数据类型值undefined
>1. Undefined类型只有一个值，即特殊的undefined
>1. 在使用var声明变量但未对其初始化是，这个变量的值就是undefined

    var message;//未对其初始化
    alert(message == undefined);//true
    var message = undefined;//没有必要这样做
    alert(message == undefined);//true
	//但是，包含undefined值得变量与定义的变量还是不一样的
	var message；//undefined
	//var age;并没有声明
	alert(message);// undefined
	alert(age);//报错
	在typeof下不会报错,没有实际意义,而在严格模式下仍然会报错
	alert(typeof message);//undefined
	alert(typeof age );//undefined

