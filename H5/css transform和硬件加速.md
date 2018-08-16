## css transform和硬件加速
[TOC]
### transform 2D变换
#### rotate/scale
- rotate旋转，只有一个参数
- - 单位角度：deg
- scale缩放
- - 一个参数：水平和垂直同时缩放 
- transform:scale(1.1)
- - 两个参数：第一个参数指定水平方向的缩放倍率，第二个参数指定垂直方向的缩放倍率。 
- - 可以是负数，如果是负数则会图片翻转
#### translate/skew
- translate位移
- - 一个值表示X轴的位移，两个值表示X轴Y轴
- -skew倾斜
- - 一个参数时：表示水平方向的**倾斜角度**；
- - 两个参数时：第一个参数表示水平方向的倾斜角度，第二个参数表示垂直方向的倾斜角度。 
- - 水平方向对应垂直方向的角，垂直方向对应水平方向的角 
- - skew的默认原点`transform-origin是这个物件的中心点`
变换基点
#### perspective-origin/transform-origin
- transform-origin
- - 默认基点为中心点，可以通过关键词设置坐标值或关键词改变基点
- perspective-origin** : `景深基点`** 在什么方位去看(`top left 左上方`)
多方法组合变形
**注:  上面我们介绍了使用transform对元素进行旋转、缩放、倾斜、移动的方法，这里讲介绍综合使用这几个方法来对一个元素进行多重变形。**
#### 用法

```
transform: rotate(45deg) scale(0.5) skew(30deg, 30deg) translate(100px, 100px);
```

- 这四种变形方法顺序可以随意，但不同的顺序导致变形结果不同，原因是变形的**`顺序是从左到右依次进行`**
- - 这个用法中的执行顺序为`1.rotate 2.scalse 3.skew 4.translate `
- - 并且，每个变形之间用“空格”分隔符，而不是“，”。

### transform 3D变换
#### 景深/transform-style/preserve-3d
- 变换风格`transform-style`
- - flat：没有3D效果。不是默认值。这个值js改变值的时候用
- `preserve-3d`：子元素将有3D的效果
- `perspective` : 景深，近大远小
#### 使用技巧
- 景深给爷爷，风格给父亲
- 即div>ul>li`(这样的结构)`
- div (`perspective`) > ul(`transform-style`) > li(其他样式`表现效果`)

#### 3D 属性
- `3D位移`：CSS3中的3D位移主要包括`translateZ()`和`translate3d()`两个功能函数；
- `translate3d(tx,ty,tz)`
- 其属性值取值说明如下：
```
tx：代表横向坐标位移向量的长度； 
ty：代表纵向坐标位移向量的长度； 
tz：代表Z轴位移向量的长度。此值不能是一个百分比值，如果取值为百分比值，将会认为无效值。
```
#### 3D旋转
- `CSS3中的3D旋转主要包括rotateX()、rotateY()、rotateZ()和rotate3d()四个功能函数`
#### scale3d
- scale3d(sx,sy,sz)

```
sx：横向缩放比例； 
sy：纵向缩放比例； 
sz：Z轴缩放比例；
```

#### 3D缩放
- `CSS3中的3D缩放主要包括scaleZ( )和scale3d( )两个功能函数；
rotate3d(x,y,z,a)`

- - `x`：是一个0到１之间的数值，主要用来描述元素围绕X轴旋转的矢量值； 
- - `y`：是一个０到１之间的数值，主要用来描述元素围绕Y轴旋转的矢量值； 
- - `z`：是一个０到１之间的数值，主要用来描述元素围绕Z轴旋转的矢量值； 
- - `a`：是一个角度值，主要用来指定元素在3D空间旋转的角度，如果其值为正值，元素顺时针旋转，反之元素逆时针旋转。

#### 三个旋转函数功能等同

```
rotateX(a)函数功能等同于rotate3d(1,0,0,a) 
rotateY(a)函数功能等同于rotate3d(0,1,0,a) 
rotateZ(a)函数功能等同于rotate3d(0,0,1,a)
```

#### 3D矩阵
CSS3中3D变形中和2D变形一样也有一个3D矩阵功能函数matrix3d()
### 开启硬件加速
- transform可以进行硬件加速,性能较好
- 具体是给浏览器发送一个css命令,让浏览器开启硬件速功能,GPU渲染能力,使动画很流畅,不会出现卡顿现象、残影、回流问题等问题的出现
- 要给运动的元素开启硬件加速    只需添加 : 

```
transform : translateZ( 0 )/transform: translate3d(0,0,0)
transform-style : perseve-3d
backface-visibility : hidden;
```