## jQuery的`css`样式
**`$("").css({})内可传json或者单个样式`**

	  $("#box").css({
          width : 100,
          height : 100,
          background : "green",
          border : "1px solid"
       })

**`hide()`  隐藏显示的元素**
**`show()`  宽高透明度会变化,没有参数代表瞬间显示,可以传回调函数等**
**`toggle()` 宽高也会随着时间的变化而变化**
**时间：可选参数  可以是代表毫秒的数字，`可以是 "fast" "normal" "slow"`**
**速度曲线：可选参数 ` "linear" "swing"`**
**回调函数：可选参数 `函数`，动画完了之后做啥**
`这三个的缺点很明显,所以用fadeIn()`
> + `fadeIn()`把隐藏的元素显示
> + `fadeOut()`把显示的元素隐藏
> + `fadeToggle()`

**`delay()`延迟,多长时间才开始做.......**

    <div id="box"></div>
    <script>
          /*
          * hide  隐藏显示的元素
          * show  宽高透明度会变化,没有参数代表瞬间显示,可以传回调函数等
          * toggle 宽高也会随着时间的变化而变化
          * 时间：可选参数  可以是代表毫秒的数字，可以是 "fast" "normal" "slow"
          * 速度曲线：可选参数  "linear" "swing"
          * 回调函数：可选参数 函数，动画完了之后做啥
          * */
          //这三个的缺点很明显,所以用fadeIn()
          /*
          * fadeIn()把隐藏的元素显示
          * fadeOut()把显示的元素隐藏
          * fadeToggle
          * */
          $(document).click(function(){
              //$("#box").show();
              //$("#box").toggle(3000);
              $("#box").hide();
          })
          //$("#box").fadeIn(3000);
          //$("#box").fadeOut(3000);
          $("#box").fadeToggle(3000);
          //延迟delay()
          //stop()可以结束动画,如果有队列,则先清空队列,在停止对话,当然也可以用来清空队列(队列名称)
      </script>

### 各种宽高
**`offset()`**
**获取/设置 元素距离 文档 的位置**
**包含`margin`**
**返回一个对象**

     console.log($("#box").offset());//返回一个对象
**滚动距离  `scrollTop()`   `scrollLeft()`**

**获取`计算`后的样式**
**`innerHeight()  innerWidth()`**
> + 获取/设置计算后样式  padding+样式
> + 设置时 `100 = width/height + padding-left+padding-right`
> + 然后改变height/width的值

**`outerHeight(),outerWidth()`**
> + 获取/设置计算后的样式  padding+样式+border
> + 设置时`100 = width/height + padding-left+padding-right +border-right+border-left`
> + 然后改变height/width的值

    <style type='text/css'>
        #box{
             position: absolute;
             width: 100px;
             height: 100px;
             margin-left: 100px;
             padding: 10px;
             background: green;
             border : 1px solid red;
         }
     </style>
     <div id="box"></div>
        <script>
            /*
            * offset
            * 获取/设置 元素距离 文档 的位置
            * 包含margin
            * position
            * margin不会影响返回值
            * 不可设置top和left值,但可以获取
            * */
            console.log($("#box").offset());//返回一个对象
            $("#box").offset({
                left : 100,
                top : 100
            });
            $("#box").position({
                left : 100,
                top : 100
            });
            console.log($("#box").position());
            //滚动距离  scrollTop   scrollLeft
            document.onclick = function(){
                //获取
                // console.log($(document).scrollTop());
                //设置
                $(document).scrollTop(300);
                console.log($(document).scrollTop());
            };
            //innerHeight()  innerWidth()
            /*
            * 获取/设置计算后样式  padding+样式
            * 设置时 100 = width/height + padding-left+padding-right
            * 然后改变height/width的值
            * */
            console.log($("#box").innerHeight());
            $("#box").innerHeight(100);
            //outerHeight(),outerWidth()
            /*
            * 获取/设置计算后的样式  padding+样式+border
            * 设置时100 = width/height + padding-left+padding-right +border-right+border-left
            * 然后改变height/width的值
            * */
            console.log($("#box").outerHeight());
            $("#box").outerHeight(150);
        </script>

**`slideDown() `向下滑动,需要display:none**
**`slideUp()  `向上滑动,不需要display:none**
**`slideToggle()`**
**可以规定时间,也可以用slow,fast,normal以及速度曲线：可选参数  "linear" "swing"**
**还有回调函数**

     $(document).click(function(){
         $("#box").slideDown();
     });

### animate
**`animate({},time)`;`time`运动所需的时间,`{}josn`,需要改变的值**
**有队列,即先执行先注册的**

    $("#box").animate({
	    left: 1000
     },1000);
     $("#box").animate({
         top: 500
     },1000);
    