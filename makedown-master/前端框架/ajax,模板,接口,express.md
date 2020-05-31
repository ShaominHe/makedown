### ajax

ajax的优点：
实现局部刷新，节省带宽资源

ajax:

```
地址,
方式：get、post、delete、put
post参数的数据格式：json 、urlencoded、formdata
请求的参数,
接受的参数  xml、json、string
```

同源策略：
同协议，同端口，同域名
比较的是当前的地址与请求的地址
解决跨域：
1、jsonp
2、服务器允许
3、反向代理

服务器允许：

```
如果是fromdata形式发送的时候可以不设置请求头，
app.all("*",function(req,res,next){
	//加入白名单，代表允许任意域名跨域
	res.header("Access-Control-Allow-Origin","*");
	//是否允许头部信息包含content-type类型   token令牌  authorization
	res.header("Access-Control-Allow-Headers","content-type,authorization")
	res.header("Access-Control-Allow-Methods","GET,POST,DELETE,PUT")
	next()
})
```

或者不让他跨域，让他符合同源策略

ajax

```
const xhr = new XMLHttpRequest();
xhr.open("get","http://127.0.0.1/reg");
xhr.send();
xhr.onreadystatechange=function(ev){
	if(xhr.readyState===4){
		if(xhr.status===200){
		
		}
	}
}

优化为：
const xhr = new XMLHttpRequest();
xhr.open("get","http://127.0.0.1/reg");
xhr.send();
xhr.onload=function(ev){
	
}
```

### 模板

```
百度模板

1、引入模板引擎
<script src="../baiduTemplate.js"></script>
2、创建模板
<script type="text/html" id="tp"> //里面写标签
	<div> 
		我在马路边捡到<%=num%>分钱
	</div>
</script>
3、使用模板
<script>//第一个元素是模板的id名，第二个元素必须是一个对象
document.querySelector("#one").innerHTML=baidu.template("tp",{num:1})
</script>


<%= %>  输出
<%%>  写js代码
```

### 接口

```
思路
1、创建接口
	//在server中写个路由
	//目的就是讲用户的信息进行输出
	1、将json进行读取
	2、将结果输出
2、调用接口
3、将得到的数据进行渲染

需要知道：
1、请求的地址：http://127.0.0.1/userList
2、请求的方式：get
3、请求的参数：无
4、返回的结果：{ok:1;userList}
```

接口例子：

![1570806491039](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1570806491039.png)

express中的接口

get:

![1570806530818](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1570806530818.png)

post:

![1570806548132](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1570806548132.png)

### express

是node.js中的一个框架。
引擎>框架>类库，jquery是一个类库
类库：在项目中为你实现某个功能提供了方法
框架：为你完成某一部分的功能，架构
引擎：为你将项目写好,如：<https://www.egret.com/>

express：
1、静态资源的处理。
2、设置路由，以及参数的处理，返回的结果

第一块：

```
//下载：
cnpm install  express  -S
// 引入express
const express = require("express");
// 执行express函数，并函数的返回值赋值给app
const app = express();
// 以get形式访问地址  /my
app.get("/my",function (req,res) {
    res.end("my");
})
// 自上而下执行，当遇到第一个满足条件的路由时，不会继续向下执行。
app.get("/you",function (req,res,next) {
    // next :是一个函数。当执行该函数以后，程序会继续向下查找满足地址的路由。
    // res.end("youyou")    //结果为输出youyou,不在向下执行
    console.log(111111111111);
    next();   //结果为输出you  控制台打印11111111111
})
app.get("/you",function (req,res) {
    res.end("you");
})

app.listen(8090,function () {
    console.log("success")
})


```

第二块：

```

const express = require("express");
const app = express();
//http://127.0.0.1:8090/sum?a=1&b=2
app.get("/sum",function(req.res){
//将问好后面的值输出为{a:'1',b:'2'}
	console.log(1111111,req.query)
	//req.query可以接受地址
	res.end()
})
//http://127.0.0.1:8090/sum/1/2
app.get("/sum/:id/:type",function(req,res){
	console.log(2222222,req.params)
	//将/后面的第一个值给id，第二个值给type,id和type可以随便起，结果为{id:'1',type:'2'}
	res.end();
})
app.listen(8090,function(){
	console.log("Success")
})
```

第三块：

```
res.send()        res.json()       res.end()

res.json()  //用于返回json数据，并结束响应
res.send()	//用于json字符串，并结束响应
res.end()   //当只是用于结束响应时使用
```

第四块：静态资源

```
//将当前木目录下的html文件夹作为你的静态资源
app.use(express.static(__dirname+"/html"));
//设为资源之后，可以直接访问文件夹下的内容
```

第五块     以post访问

```
get在地址栏传递的参数，get也可以接收
post传值
前端：怎么传？
	1、json
		//json传的值是必须是json字符串
		xhr.setRequestHeader("content-type","application/json");
        xhr.send(JSON.stringify({num:num.value,num2:num2.value}));
	2、urlencoded
	3、formdata
后端：怎么收？
1、下载
	cnpm install body-parser -S
2、引入
	const bodyParser=require("body-parser")
3、设置
	第一种：将post过来的数据放置到req.body当中，且要求传递过来的数据格式为json
	app.use(bodyParser.json())
	第二种：将post过来的数据放置到req.body当中，且要求传递过来的数据格式为urlencoded
	app.use(bodyParser.urlencoded({
         extended:false// 是否深度解析。true是，false否
    }));
    4、使用
    req.body
```

补充：

urlencoded格式深度解析的区别

![](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1570805506083.png)

false:

![](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1570805480643.png)

true:

![](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1570805618498.png)

例：

```
const express=require("express");
const bodyParser=require("body-parser")
const app=express();
// 设置为json格式
// app.use(bodyParser.json())
//设置为urlencoded格式
app.use(bodyParser.urlencoded({
    extended:true
}))

app.use(express.static(__dirname+"/html"))
app.post("/sum",function(req,res){
    const {num,num2}=req.body;
    const sum=num/1+num2/1
    res.json({
        ok:1,
        sum
    })
})
app.listen(80,function(){
    console.log("success")
})
```

### 细节：

地址是写相对还是绝对