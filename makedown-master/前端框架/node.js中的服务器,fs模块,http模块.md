### node.js

node.js是一个基于v8引擎，用于运行javascript的环境
前后端分离写法：
服务器：是一个电脑主机，安装软件 phpstudy，运行你的站点，搭建一个web服务
前端：调用接口，然后将接口提供的数据以合理的方式呈现出来
后端：提供接口，对数据进行操作（增，删，改，查）
项目：运行在你的服务器当中
数据库：存放数据

node-v：查看版本号
node :进入环境
ctrl+c:退出环境
cls:清屏
上下箭头操作之前的命令
tab:可以切换文件

控制台：
window+r 
shift+右击
文件地址中输入cmd

#### <font color="red">创建服务</font>

http(80)   https(443)

##### 引入模块（依赖，模组）

三种模块：内置模块、自定义模块、第三方模块

```
//require表示引包，引包就是引用自己的一个特殊功能
const http=require("http")   
console.log(typeof http)    //是个对象
const server =http.createServer(function(req,res){
//req表示请求，request； res表示响应，response
//设置HTTP头部，状态码是200，文件类型是html，字符集是utf8
res.writeHead(200,{"content-type":"text/html;charset=UTF-8"})
//文档类型也可以是文档格式，些什么就是什么
res.writeHead(200,{"content-type":"text/plain;charset=UTF-8"})
//结束响应，输出内容，只支持字符串
response.end("<a href='www.baidu.com'>123123</a>") 
})
//运行服务器，监听3000端口（端口号可以任意改）
server.listen(3000,"127.0.0.1",function(){
	//当服务创建完毕以后，会执行该函数
	console.log("服务创建完毕")
})   //可以直接链式操作跟在后面
```

当你的服务启动之后（node xxx.js），可以通过你的电脑来进行访问该服务（web站点服务）
http://127.0.0.1       或者  http://10.9.27.22  利用ipconfig查看
当修改node 代码之后，要重启。

只能运行一次，否则会报错

```
listen EADDRINUSE: address already in use :::3000   //端口号被占用
```

##### fs模块

```
const fs=require("fs");
//第一个参数是文件地址，第二个参数是回调函数（当读取数据完毕之后执行。）
//读取文件
fs.readFile("./a.js",function(err,results){   //异步操作
	 if(err){
            return err;
        }else{
            return results.toString();
        }
})      
```

```
const fs=require("fs")
function fn(){
    fs.readFile("./a.js",function(err,results){
        if(err){
            return err;
        }else{
            return results.toString();
        }
    })
}
fn()   //因为fs.readFire是异步操作会导致输出结果为undefined

所以解决方法一：（利用回调函数）
const fs=require("fs")
function fn(cb){
    fs.readFile("./a.js",function(err,results){
        if(err){
            return err;
        }else{
            cb(results.toString())
        }
    })
}
fn(function(data){
    console.log(data)
})
```

###### fs写入文件

注意：

<font color="red">写入的是字符串</font>

```
第一个参数写入文件的地址，第二个参数是要写入的内容，第三个参数是{flag:"w"}或者是{flag:"a"}默认是w,w是覆盖,a是追加，可省略，第四个是回调函数
fs.writeFile(_dirname+"/day2/data.json","啊啊啊",{flag:"w"}function(err){
	console.log(err);
})
```

例：

```
const obj={
	username:"张三",
	age:12,
	sex:"男"
}
fs.writeFile(_dirname+"/day2/data.json","JSON.stringify(obj)",function(err){
	console.log(err);
})
```



##### 单线程

```
const http = require("http");
const fs = require("fs");
// .net c#  java   node
const server = http.createServer((req,res)=>{
    console.log(11111111111);
    fs.readFile("./8、fs.js",function (err,results) {
        console.log(2222222222);
        res.end(results.toString())
    })
})
//
server.listen(8090,function () {
    console.log("服务创建完毕1")
})
```

当访问的人数次数多的时候可能会出现多个11111连在一起或者多个22222连在一起

##### node.js是没有文件夹概念的

```
const http = require("http");
const fs = require("fs");
const server = http.createServer((req,res)=>{
    if(req.url.toLowerCase() === "/page/index.html" ){ //toLowerCase()都转为小写
      //只要满足条件，不在同一个文件夹下也可以读取
        fs.readFile("./page/index.html",(err,results)=>{
            res.end(results.toString());  
        }) //但是读取不到css文件，css文件走的是404,必须在做判断
    }else{
        res.end("404")
    }

})
//
server.listen(8090,function () {
    console.log("服务创建完毕1")
})
```

```
const http = require("http");
const fs = require("fs");
const server = http.createServer((req,res)=>{
    if(req.url.toLowerCase() === "/page/index.html" ){
        fs.readFile("./page/index.html",(err,results)=>{
            res.end(results.toString());
        })
    }else if(req.url.toLowerCase() === "/page/style.css" ){ 
    //此时既可以读取到html文件也可以读取到css文件
        fs.readFile("./page/style.css",(err,results)=>{
            res.end(results.toString());
        })
    }
    else{
        res.end("404")
    }

})
//
server.listen(8090,function () {
    console.log("服务创建完毕1")
})
```

### 路由

创建服务器的例子

```
const http=require("http")
const fs=require("fs")
const server=http.createServer((request,response)=>{
    console.log(request.url)  //request.url是两次请求，一次内容，一次战标
    if(request.url === "/index1.html"){		//路由
        fs.readFile("./index1.html",function(err,results){
            response.end(results.toString())
        })
    }else if(request.url === "/style.css"){
        fs.readFile("./style.css",function(err,results){
            response.end(results.toString())
        })
    }else{
        response.end("404")
    }
       
    
})
server.listen(3000,function(){
    console.log("服务创建完毕")
})
```

