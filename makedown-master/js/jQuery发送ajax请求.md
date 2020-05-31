### jQuery发送ajax请求

##### get请求

```
$.get(url,[data],[callback],[type])
url：地址
data：参数
callback：成功后的回调函数
type:返回内容的格式，xml,html,script,json,text,_defalut
```


如果获取到的是字符串使用responseText
如果获取到的是xml的话就使用responseXML

##### post请求

$.post(url,[data],[callback],[type])

##### ajax请求（通用的）

```
$.ajax({

​	url:"ajax.php",  		//请求地址
​	async:true,				//是否异步
​	data:{name:"张三"},		//传递的数据
​	type:"post",				//请求方式
​	dataType:"json",		//希望返回的数据类型
​	success:function(res){		//请求成功调用的函数

​			console.log(res)

​	},
​	timeout:1000,		//限定请求完成所需要的时间，单位毫秒，如果超出这个事件还没有请求成功，就报错  timeout
​	error:function(ajax,err){	//请求错误的时候，调用这个方法打印一些错误的信息

​			console.log(ajax)		//ajax是当前的ajax对象

​			console.log(err)		//err错误信息

​	}
​	context:$(指定元素)		//将当前对象中的this改为指定对象
​	cache:false			//是否缓存，默认true缓存

})
```



##### jsonp请求

跨域：协议、域名、端口号有一个不一样的，就造成了跨域
跨域方法：
在后端设置响应头，允许别人跨域访问
利用标签的属性没有跨域限制：
link标签的href属性---将请求来的数据当做css代码执行
img的src属性--将请求来的数据当做图片显示
script标签的src属性--将请求来的数据当做JS代码执行----jsonp
iframe标签的src属性--将请来的数据当作html内容解析
缓存：为了多次的请求效率，请求地址后面跟一个时间戳，表示每一次的请求地址是不同的，解决了缓存问题

```
$ajax({	//默认在请求的时候会带一个时间戳

​	url:请求地址,
​	dataType:'jsonp',
​	data:{name:'jack',age:18},
​	success(res){

​		console.log(res)

​	},
​	jsonp:'cb',	//jsonp请求的时候回调函数的key
​	jsonpCallback:'fn', 	//jsonp请求的时候回调函数的名称

})
```

jsonp默认get请求
php接受的时候必须是$_GET["callback"]
![1568453468138](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568453468138.png)



##### 全局ajax函数

<font color=red>只针对jquery的ajax请求</font>

###### 钩子函数：

当某件事情执行到一定的阶段的时候，拦截住，调用一个函数-监控到这个操作的每个阶段，在这个操作进行到某个阶段的时候可以做另一个事情，这个函数就叫钩子函数

全局ajax函数就是ajax执行过程中会触发的一些钩子函数
当前页面第一个ajax准备开始的时候触发的函数：
$(window).ajaxStart(function(){

​	console.log("ajax即将开始请求")

})
当每个ajax发送请求的时候，---send()会触发钩子函数
$(window).ajaxSend(function(){

​	console.log("当前请求马上发送")

})
放每个ajax请求成功的时候触发的钩子函数
$(window).ajaxSuccess(function(){

​	console.log("当前ajax请求成功")

})
当每个ajax请求失败的时候会触发的钩子函数
$(window).ajaxError(function(){

​	console.log("当前ajax请求失败")

})
当每个ajax请求完成的时候会触发的钩子函数
$(window).ajaxComplete(function(){

​	console.log("当前ajax请求完成")

})
当前页面最后一个ajax请求结束的时候，会触发的钩子函数
$(window).ajaxStop(function(){

​	console.log("最后一个ajax请求结束")

})



##### jquery的标识符

 jquery的开头都是$或jquery获取，如果在页面引入别人插件，别人插件写的也是要用$或jquery开头的，那么会和引入的jquery产生冲突，jquery早已预料到了这种情况，所有jquery可以将$或jQuery的使用权交出，或换成其他的操作符
jquery.noConflict(); 	//将$符的使用权交出，$符不能使用了，只能使用jquery
jquery.noConflict(true);	//交出了$和jquery的使用权，$符号和jquery都不能使用了
var  变量=jquery.noConflict(true);  //使用自定义的变量代替$和jquery



jquery扩展

jquery提供了两套方法：
1、给元素集合使用
2、给jquery自己使用

给元素集合使用
例：做一个全选的

![1568456509456](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568456509456.png)

给jquery自己扩展方法
![1568456740126](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568456740126.png)



##### jquery插件

jquery之家
www.jq22.com
swipper:轮播图
懒加载：图片出现在可是区域的时候开始加载
表单验证：专门用来验证表单的
分页插件