## jQuery

官网：jquery.com
中文方法：jquery.cuishifeng.cn
在线网站：<https://www.bootcdn.cn/>
jQuery是js的库，也是使用最多的库。原因是：
1、强大的选择器机制
2、优质的隐式迭代
3、无所不能的链式编程
`要是用query首先需要下载一个jQuery文件，然后先引入才能使用`
首先检测一下有没有引入成功，就是输出$或者jQuery
jquery任何操作都以$或者jQuery开头
整个jQuery就是一个函数返回的是一个函数：window.jQuery=window.$=jQuery

#### 选择器

###### 1、在jQuery中选择元素

$div = $ ("div")或者 $div = jQuery("div")
jquery选择的元素永远是伪数组--不能使用js原生操作---除非取下标获取到准确的dom元素
例如：div.innerText='box'     会报错
			div[0].innerText='box' 或者
			div.get(0).innerText='《b>文字《/b>'
原生js的dom对象也不能使用jquery提供的方法---除非将dom元素转为jquery对象--直接$(div)就转了
例：var div=document.querySelector("div")
		$(div).text("文字" );

###### 2、<font color=red>css选择器</font>

（1）标签选择器------$("标签名")
《div>《/div>
var div = $("div")
（2）类名选择器------$(".类名")
《div class="box">《/div>
var box = $(".box")
（3）id选择器------$("#id名")
《div id="myid">《/div>
var myid = $("#myid")
（4）属性选择器------ $("[name='username']")
《input type = "text">
var  text = $("[type = 'text']");

###### 3、伪类选择器（表单的伪类）

<table border=1 width=500>
    <tr>
        <th>姓名</th>
        <th>年龄</th>
        <th>成名绝技</th>
    </tr>
    <tr>
        <td>令狐冲</td>
        <td>20</td>
        <td>独孤九剑</td>
    </tr>
    <tr>
        <td>东方不败</td>
        <td>48</td>
        <td>葵花宝典</td>
    </tr>
    <tr></tr>
    <tr>
        <td>任我行</td>
        <td>55</td>
        <td>吸星大法</td>
    </tr>
    <tr>
        <td>杨过</td>
        <td>40</td>
        <td>黯然销魂掌</td>
    </tr>
</table>
（1）获取第一个元素
var firstTr=$("tr:first-child");
console.log(firstTr)
（2）获取最后一个元素
var lastTr = $("tr:last-child")
（3）获取第几个元素
var  secondTr=$("tr:nth-child(2)")
（4）索引为奇数的tr
var evenTr=$("tr:even")
（5）索引为偶数的tr
var oddTr=$("tr:odd")
（6）empty 指的内容为空的元素
var emptyTr=$("tr:empty")

###### 4、筛选器

（1）选择第一个元素
$("tr:first")
（2）选择最后一个元素
$("tr:last")
（3）选择奇数元素
$("tr:even")
（4）选择偶数元素
$("tr:odd")
（5）下标大于指定数字的所有元素
$("tr:gt(下标)")
（6）下标大于指定数字的所有元素
$("tr:lt(下标)")
6）下标等于指定数字的所有元素
$("tr:eq(下标)")

###### 5、表单元素选择器--了解

$(":input") // 匹配所有的表单元素 包括：文本框（input）下拉列表(select)、文本域(textarea) 

$(":text") // 匹配单行文本框type="text" $("input:text") $("input[type=text]") 

$(":password") // 匹配单行密码框 

$(":radio") // 匹配单选按钮 

$(":checkbox") // 匹配多选按钮 

$(":submit") // 匹配提交按钮 

$(":reset") // 匹配重置按钮 

$(":image") // 匹配图片按钮 

$(":button") // 匹配普通按钮 

$(":file") // 匹配文件上传 

$(":hidden") // 匹配隐藏域



###### 6、<font color=red>表单对象选择器</font>

$("input:enabled")	//所有可用的表单元素
$("input:disabled")	//所有禁用的表单元素
$("input:checked")	//<font color=red>所有选中的表单元素---重点</font>
$("select:selected")	//<font color=red>被选中的下拉框元素----重点</font>

###### 7、<font color=red>筛选器方法----重点</font>

$("li").first() // 第一个元素 

$("li").last() // 最后一个元素 

$("div").eq(下标) // 指定下标的div元素 

$("div").next() // div的下一个兄弟元素 

$("div").prev() // div的上一个兄弟元素 

$("div").nextAll() // div后面的所有兄弟元素 

$("div").prevAll() // div前面的所有兄弟元素 

$("div").parent() // div的父元素 

$("div").parents() // div的所有直系祖宗元素 

$("div").find(选择器) // 从父元素中找到符合选择器的子元素

$("div").siblings() // div的所有兄弟元素

end()   //返回上一次操作的对象



###### 8、事件 

将事件名作为方法名click、dblclick、mouseover、mouseout、focus、blur、submit......
元素 . 事件类型(function(){

})
例：$("div") . click(function(){

})$("div") . mouseover(function(){

})

页面加载事件 
在原生js中的页面加载事件是window.onload
在jquery中有两种写法：
$(function(){

});
$(document) . ready(function(){

})
jquery的相比于js原生的写法效率高，因为js元素的页面加载事件需要等到页面中的所有资源加载完毕后才执行，而jquery的页面加载事件只需要等到页面的标签加载完毕就执行，而不会等待外部文件加载

事件处理：
（1）on方法用于绑定事件、委托事件、传入参数：
$(元素) . on(事件类型 , [委托元素] , [委托元素] , [处理的函数的参数]，事件处理函数)    [ ]代表可以省略   `jquery中的事件对象e不存在兼容问题`

![1568118858564](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568118858564.png)
（2）off方法用于解绑事件：
$(元素).off(事件类型，处理函数)
（3）trigger方法用于手动触发事件：
$(元素).trigger(事件类型,处理函数)
（4）只能触发一次的事件,然后自动销毁：
$(元素).one(事件类型,处理函数);
（5）特殊事件：
 hover事件，包含鼠标放上去和鼠标离开 ：
$("元素").hover(鼠标放上去的处理函数,鼠标离开的处理函数)



###### 9、属性操作

设置属性：
$("div").prop(属性名,属性值)
获取属性：
$("div").prop(属性名)   //复选框checked选中是true选不中是false
设置自定义属性：
$("div").attr(属性名，属性值)
获取自定义属性：
$("div").attr(属性名)
删除属性：
$("div").removeProp(属性名)
$("div").removeAttr(属性名)    //删除自定义属性

###### 10、隐式迭代

jquery中的数组不需要遍历就可以使用
![1568121861932](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568121861932.png)



###### 11、操作内容

js操作内容：
innerText
innerHtml
value
jquery操作内容：
元素 . text( );
元素 . html( );
元素 . val( );----操作表单的内容
如果在方法中不传参数，就是在获取元素的内容
如果传入参数，就将元素的内容设置为参数
获取元素的文本内容：元素.text();
设置元素的文本内容：元素.text("要设置的内容")
获取元素带标签的内容：元素.html();
设置元素带标签的内容：元素.html("要设置的内容")
获取表单元素的内容：元素.val()
设置表单元素的内容：元素.val("要设置的内容")

###### 12、样式操作：css方法：元素.css( )

获取样式：
$("div").css("color")

设置一个样式：
$("div").css("color","red")

设置多个样式：
$("div").css({
	width:200,     //不用单位  默认px
	height:200,
	background:"yellow",

})

###### 13、类名操作

删除类名：removeClass("要删除的类名")
添加类名：addClass("要添加的类名")  
让类名在添加和删除之间切换：toggleClass("要切换的类名")
判断元素是否有指定的类名：hasClass("要检测的类名")---返回布尔值
![1568123643874](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568123643874.png)


补充：

index()-