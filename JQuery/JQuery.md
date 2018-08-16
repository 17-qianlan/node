## JQuery
[jQuery的知识点链接](http://jquery.cuishifeng.cn/id.html)
### Jq和DOM的互相转化
**`jq转化为DOM ->   $(select).get(index);index必须`**
**`DOM转化为jq ->   $(ele).html();`**

     //DOM转化为jq
     var oBox = document.getElementById("box"),
         oSpan = oBox.getElementsByTagName("span")[0];
     $(oSpan).html("太对了");
     $(oSpan).css({
         display: "block",
         width:300,
         height:300,
         background:"green",
         color:"red"
     });
     //jq转化为DOM
     var oSpan = $("#box").get(0);
     oSpan.innerHTML = "5555";
### 选择器
**`$("select,select")里可多选,多写`**
**`+`表示兄弟选择器**
**`:even选择偶数`**

**children()**；
>+ 所有后代，不包括后代的后代
>+ 只有标签


**获取焦点事件`.focus( )`**
**`:focus`要有焦点才会有作用,`匹配具有焦点的元素`**
**`:root`选择该元素的根元素,是`html的根元素`**
**`this`会冒泡点击不到li而event.target可以点击到li(`不会冒泡`)  -> e.target**

    <div id="box">
            我是boss
            <p>
                我是p标签
                <span>我是span便签</span>
            </p>
        </div>
        <ul>
            <li>我是ul下的li0</li>
            <li>我是ul下的li1</li>
            <li>我是ul下的li2</li>
            <li>我是ul下的li3</li>
            <li>我是ul下的li4</li>
            <li>我是ul下的li5</li>
        </ul>
        <b>我是b</b>
        <a href="">
            我是a
        </a>
        <form action="">
            <input type="text" />
        </form>
        <script>
            //$("select,select")里可多选,多写
            $("#box,a").css("color","red");
            //+表示兄弟选择器
            $("#box+ul").css("border","1px solid #000");
            //:even选择偶数
            console.log($("li:even").css("color","red"));
            //获取焦点事件.focus()
            $("input").focus().css({
                width: 500
            });
            //:focus要有焦点才会有作用,匹配具有焦点的元素
            $("input:focus").css({
                background:"red"
            });
            //:root选择该元素的根元素,是html的根元素
            $(":root").css({
                background : "white"
            });
            //this会冒泡点击不到li而event.target可以点击到li(不会冒泡)  -> e.target
            $("div,b,ul").click(function(e){
                console.log($(e.target));
            })
        </script>

### 属性
**`$().attr()`**
> + 有是获取,没有是添加,可以是一个json
> + 自带遍历
> + removeAttr 移出属性

**遍历`each()`**
**`$().prop()`中的checked,只能用`prop`,只能操作`合法`属性**
**`toggleClass()`,有添加,没有删除**

    <div id="box">
         <ul class="ls">
             <li class="list_0"></li>
             <li class="list_1"></li>
             <li class="list_2"></li>
         </ul>
         <div class="box"></div>
     </div>
     <input type="checkbox" checked="" disabled="disabled"/>
            /*
            * $( ).attr();
            * 有是获取,没有是添加,可以是一个json
            * 自带遍历
            * removeAttr 移出属性
            * */
            $("#box,.box").attr("ccc","222");
            //$().prop()中的checked,只能用prop,只能操作合法属性
            $("input").prop("checked",false);
            $("input").prop("ccc",222);//无效
            //toggleClass,有添加,没有删除
            $("ul").toggleClass("ls");

### 文档处理
**`添加标签,也可插入innerHTML`**
> + `$("#box").append("<i></i>")`

**appendTo()   $(A).appendTo(B)   把A追加到B中  不可以新建标签**
**`$(A).prepend($(B)) `  向每个匹配的元素插入"内部"前置  把B插入A中**
**`$(A).prependTo($(B)) 向每个匹配的元素插入"内部"前置  把A插入B中`**
**after  $(A).after(B)   把B插入A后面**
**$(A).insertAfter(B)    把B插入A前面**
**`before  $(A).before(B)`   把B插入A前面**
**`$(A).insertBefore(B)    把B插入A后面`**

    <div id="box"></div>
        <p class="p">
            <i></i>
            <b></b>
        </p>
        <span></span>
        <a href=""></a><script>
            //添加标签,也可插入innerHTML
            //$("#box").append("<i></i>");
            $("#box").append("2222");
            //appendTo()   $(A).appendTo(B)   把A追加到B中  不可以新建标签
            $("span").appendTo(".p");
            //$(A).prepend($(B))   向每个匹配的元素插入"内部"前置  把B插入A中
            //$(A).prependTo($(B)) 向每个匹配的元素插入"内部"前置  把A插入B中
            $("i").prepend($("b"));
            $("span").prependTo($("a"));
            //也可以
            $("span").prependTo("a");
            //after  $(A).after(B)   把B插入A后面
            //$(A).insertAfter(B)    把B插入A前面
            $("#box").after("<strong></strong>");
            //before  $(A).before(B)   把B插入A前面
            //$(A).insertBefore(B)    把B插入A后面
            $("#box").before("<b></b>");
        </script>
**`$(A).wrap(B)`   把A打包进入B,且复制A一次**
**`unwrap    和wrap()相反`**
**`wrapAll() ` 可以传一个类数组,把所有都包裹起来,也可以是一个单标签**
**`wrapInner()`把标签内的"内容"用一个标签包裹起来,也可用来包裹标签**
**`replaceWidth()`把指定的标签换成指定的HTML和DOM**
**`replaceAll()`把所有的selector替换**
**`empty()`删除匹配的元素集合中所有的"子节点"**
**`remove()`移出指定的元素,但在jQuery不会删除,会移出事件之类的东西**
**`detach()`移出指定的元素,但在jQuery不会删除,会移出事件之类的东西**
**`clone()`克隆指定的元素,并且Boolean内为true指示事件处理函数是否会被复制**
**指示是否对事件处理程序和克隆的元素的所有子元素的数据应该被复制。**

    <p class=""></p>
        <i></i><script>
            //$(A).wrap(B)   把A打包进入B,且复制A一次
            $("i").wrap("p");
            //unwrap    和wrap()相反
            $("i").unwrap();
            //wrapAll()  可以传一个类数组,把所有都包裹起来,也可以是一个单标签
            //wrapInner()把标签内的"内容"用一个标签包裹起来,也可用来包裹标签
            //replaceWidth()把指定的标签换成指定的HTML和DOM
            //replaceAll()把所有的selector替换
            //empty()删除匹配的元素集合中所有的"子节点"
            //remove()移出指定的元素,但在jQuery不会删除,会移出事件之类的东西
            //detach()移出指定的元素,但在jQuery不会删除,会移出事件之类的东西
            $("i").remove();
            //clone()克隆指定的元素,并且Boolean内为true指示事件处理函数是否会被复制
            //指示是否对事件处理程序和克隆的元素的所有子元素的数据应该被复制。
        </script>

### 事件
**`ready()DOM`加载完成后执行,可多写**
**`event.preventDefault()`清除默认事件**
**`on()`可以写一个或(`多个事件{}`),可以给事件类型`添加新的名称`,让off函数能够很方便的移除**
**事件委托**
**`$(selector).on([type]`,"请求委托者",fn);**
**解除事件委托 用off()**
**`off()` 在选择元素上移除一个或多个事件的事件处理函数。**
**`one()`绑定一次性事件**
**hover(onmouseenter,onmouseleave);两个处理函数移入,移出`用json`**

    <script>
            //ready()DOM加载完成后执行,可多写
            $(document).ready(function(){

            });
            //on()可以写一个或(多个事件{}),可以给事件类型添加新的名称,让off函数能够很方便的移出

            //事件委托
            //$(selector).on([type],"请求委托者",fn);
            //解除事件委托 用off()

            //off() 在选择元素上移除一个或多个事件的事件处理函数。
            $(document).on("click.goudan" , function () {
                console.log( 1 );
            });
            //绑定自定义事件
            $(document).on("click.dachui" , function () {
                console.log( 2 );
            });

            $(document).off("click.goudan");
            //one()绑定一次性事件
            //hover(onmouseenter,onmouseleave);两个处理函数移入,移出用json
        </script>

###jq的队列
[链接](https://www.cnblogs.com/snandy/archive/2013/02/18/2892749.html)
**队列 开启队列`queue()`   解除队列`dequeue()`;**
###jq拓展
**jQuery.extend**