## jQuery高级

| jquery对象==伪数组                                           |
| ------------------------------------------------------------ |
| ![img](file://D:/%E5%AD%A6%E4%B9%A0%E8%B5%84%E6%96%99/day29-jQuery%E9%AB%98%E7%BA%A7/day29-jQuery%E9%AB%98%E7%BA%A7/1-%E7%AC%94%E8%AE%B0/media/1568172945807.png?lastModify=1568186327) |

##### 补充：

return：false；可以阻止默认行为，默认事件......

#### 元素节点操作

##### 创建节点：

$("标签和内容")  里面可以加标签，属性，内容
![1568186000589](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568186000589.png)

创建的结果：

| 创建的元素                                                   |
| ------------------------------------------------------------ |
| ![img](file://D:/%E5%AD%A6%E4%B9%A0%E8%B5%84%E6%96%99/day29-jQuery%E9%AB%98%E7%BA%A7/day29-jQuery%E9%AB%98%E7%BA%A7/1-%E7%AC%94%E8%AE%B0/media/1567969959802.png?lastModify=1568186366) |

放到body中需要取下标或者$(document).find(body).append(res)


##### 添加元素：

父元素追加一个子元素：父元素.append(子元素)       		或者		   子元素.appendTo(父元素)
将元素添加到父元素的最前面：父元素.prepend(子元素)			或者		子元素.prependTo(父元素)			
添加元素到指定元素前面，成为兄弟元素：新元素.insertBefore(指定的元素)				或者        指定元素.before(新元素)
添加元素到指定元素后面，成为兄弟元素：新元素.insertAfter(指定的元素)				或者        指定元素.after(新元素)



##### 替换元素：

被替换的元素.replaceWith(新元素)  		或者     新元素.replaceAll(被替换的元素)--------这两种方法都能替换很多元素，不是只有一个



##### 删除元素：

父元素.empty()--------将子元素删掉，留下自己
父元素.remove()----------将子元素删掉，自己也删掉



##### 复制元素：

js复制元素：元素.cloneNode([true])----没有true是浅复制，有了true是深复制
jquery复制：元素.clone()-------就是深复制
元素.clone(参数1，参数2)
参数1：表示是否辅助自身的事件，true是复制，false是不复制，默认是false
参数2：表示是否复制子元素的事件，true是复制，false是不复制，默认跟随第一个参数-----但是如果设置不复制父元素的事件，第二个元素是true，也不能复制子元素的事件



##### 元素的尺寸：

获取元素尺寸：
js中是offsetWidth、offsetHeight、clientWidth、clientHeight

jquery获取尺寸：

**1、width()、height()**	

获取到的是不包含内填充的尺寸----可操作的尺寸
设置元素可操作尺寸：方法中加参数
例：$("div").width(500)

**2、innerWidth()、innerHeight()**

获取到的是包含内填充的尺寸-----可操作的尺寸+padding
不能设置尺寸

**3、outerWidth()、outerHeight()**

获取到的是包含边框和内填充的尺寸------可操作的尺寸+padding+border
可以加一个参数true  可以获取到外边距------可操作的尺寸+padding+border+外边距
也不能设置尺寸



##### 元素的位置1：

js获取元素位置：offsetLeft、offsetTop

jquery中
**获取元素位置**
**offset()**----返回的是一个对象，包含了left和top的坐标，坐标是以浏览器为参考计算的坐标
**设置元素位置**：offset(对象) 			传入一个由left和top组成的催下	参考对象：{left:值,top:值}
**例**：

```
$("div").offset({	//left值不够目标300，是在原来的基础上补到300，而不是直接加300
		left:300,   //默认单位是px	//设置值的时候，如果元素本身没加定位，会自动加上position:relative
		})
```

设置元素总结：
如果元素本身有定位，就在定位的left和top的偏移值上进行设置目标距离
如果元素本身没有定位，就在元素上自动添加相对定位，仔仔元素left和top偏移值上设置到目标的距离.



##### 元素的位置2：

获取元素位置：
position()    获取到的是自己设置过的定位偏移值跟自身在什么位置无关，只获取自己设置过得到偏移值
**注意：**
position()	其实获取到的是设置过定位的祖宗元素和自己之间的距离
如果直接在body中，获取到的是8,8因为body距离html的距离是8



##### 元素的滚动距离：

js中获取元素滚动过的距离：window.scrollTop、window.scrollLeft
jquery获取滚动过的距离：scrollTop()、scrollLeft
js中滚动事件：window.onscroll
jquery中的滚动事件：

```
$(window).scroll(function(){
		$(this).scrollTop
})
```

例：回到顶部
![1568213965978](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568213965978.png)



#### 动画

1、显示、隐藏

```
显示：
元素.show();	 // 让元素从隐藏状态变为显示状态（从display:none;变为 display:block;）
可选参数1：时间，毫秒数，（fast 400/normal 600默认/slow 800）
可选参数2：速度方式---linera(匀速)    swing(由慢到快)
可选参数3：动画结束后执行的回调函数
切换显示隐藏：
元素.toggle();      //让元素在隐藏和显示状态切换
可选参数和show一样
```

![1568215109558](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568215109558.png)



2、元素上下拉动显示隐藏：

```
元素.slideDown()		//让元素向下拉动显示
元素.slideUp()			//让元素向上拉动隐藏
元素.slideToggle()			//让元素切换上下拉动显示隐藏
参数和show一样
```


![1568215252643](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568215252643.png)



3、元素透明度显示隐藏：

```
元素.fadeIn()
元素.fadeOut()
元素.fadeToggle()
参数和show一样
让元素切换到指定的透明度：
元素.fadeTo(毫秒数，透明度，[速度方式]，[结束的回调函数])  
//让元素在指定的时间内切换到指定的透明度
```



#### 自定义动画：

开启动画
元素.animate({		**//可以进行链式操作**

​		css属性名：属性值，

​		css属性名：属性值，

}，[毫秒数]，[速度方式]，[执行结束的回调函数])；

停止动画
元素.stop();		//将动画停止在当前的状态（中止动画）
元素.finish();		//将动画停止在结束状态 （结束动画）

![1568216215832](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568216215832.png)

![1568215492254](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568215492254.png)

