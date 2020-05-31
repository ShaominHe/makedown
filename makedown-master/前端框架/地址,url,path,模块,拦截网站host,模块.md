### 地址

fs.readFile后面的地址是指node运行的地址
相对地址：是node运行的相对地址
为了避免后期运维的错误，使用**完整地址**

```
__dirname       是文件所在的文件夹地址
__filename		是文件所在的完整地址
__dirname+"/"	是根目录，盘符
__dirname+"./"	是当前目录
__dirname+"../"	是上一级目录
```

### path模块

返回上一级模块时需要用到path模块

```
path.resolve()  //两个参数
```

例：

```
fs.readFile(path.resolve(__dirname,"../three.txt"),function(err,results){
	console.log("err",err)
	console.log("results",results.toString())
}
```



### url模块

地址栏中?后面传递的是参数，参数的改变不会影响请求地址

```
解析一个地址
const str="http://10.9.27.167:8090/sum?a=1&b=4#one"//地址
consoel.log(url.parse(str))   //得到如图
consoel.log(url.parse(str).pathname) //得到/sum
consoel.log(url.parse(str).query) //得到 a=1&b=4
consoel.log(url.parse(str,true).query) //得到 [a:'1',b:'4'] 转为数组
```

![1570605977771](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1570605977771.png)

例：

```
const url=require("url")
const http=require("http")
const fs=require("fs")
const path=require("path")

const server=http.createServer((req,res)=>{
  // console.log(url.parse(req.url))
  const pathname=url.parse(req.url).pathname.toLowerCase()
  if(pathname==="/favicon.ico"){
    //战标
 fs.readFile(path.resolve(__dirname,"./u=1652432255,743536399&fm=26&gp=0.jpg"),(err,results)=>{
      res.end(results.toString())
    })
  }else if(pathname==="/"){
    // 内容
    fs.readFile("./index1.html",(err,results)=>{
      res.end(results.toString())
    })
  }else if(pathname==="/sum"){  //判断地址栏中有/sum才可以显示，如果直接req.url判断不严谨  
    const query=url.parse(req.url,true).query  //拿出?和#中间的数据，并转为数组
    const sum=query.a/1+query.b/1  // 将数字转为number并相加  如：http://127.0.0.1?a=1&b=3#one   结果为4
    res.end("success"+sum)
  }else{
    //404
    res.end("404")
  }
})
server.listen("80",function(){
  console.log("创建服务成功")
})
```

### host

```
ping 一个网址
ping 一个网址 -t    //不停的请求检测是否可以访问
```

```
可以拦截网址
打开host文件
更改host文件即可
如：
	127.0.0.1    www.1000phone.com
	127.0.0.1	 1000phone.com
	此时访问千锋官网就无法访问而会访问到自己的服务器
权限造成无法更改
	1、更改权限
	2、将文件赋值到桌面，修改，放回去
	3、下载一个软件nodepad++
```



在服务当中接受数据，并将数据进行保存，保存到data.json文件中

### 模块

1、内置模块

```
http 、url 、path、fs
```

2、自定义模块

```
导入:（依赖）
	require
导出:（暴露）
	module.exports

导入时，.js可以省略，当你是省略.js时，如果指定地址没有该js文件，那么它会找目录下的文件夹下的index.html
可以新建package.json文件，在文件中写一个入口：
{
	"main":"home.js"
}
就可以将默认的index.js改为home,js
module.exports是一个对象，可以直接.什么，也可以跟一个数组，对象，或者函数
当没有这个文件时，他首先会判断当前地址当中是否有node_module文件夹
  1、有：看node_module文件夹内是否有mo2.js
        1、有mo2.js:使用mow.js的导出内容
        2、无mo2.js：查找是否有mo2文件夹
            1、有：会使用该文件夹的index.js,向上一级，查找node_modules
            2、无：向上一级，是否有node_modules文件夹
  2、无：向上一级，是否有node_modules文件夹
```

3、第三方模块

```
npm install node_sass --save
npm i node_sass -S
npm  i install node_sass --save-dev
npm  i install node_sass -D

cnpm:
	 npm i cnpm -g
```



​		