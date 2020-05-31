### webpack

官网：http://webpackjs.com

1、打包：js丑化   html压缩    图片压缩

2、转换：sass   less   es   ts转换为js

3、优化项目：服务热更新       代理服务器

**********************************

#### 模式：mode

1、开发环境：development

2、生产环境：production

**************************************

#### 安装webpack

1、cnpm   install    webpack    -g

2、cnpm    install   webpack-cli    -g

webpack   -v	查看版本

运行webpack:webpack

黄色的是警告，红色的是异常

*********************************************************

#### webpack  0配置

默认将src文件夹下的index.js打包到dist里的main.js当中

打包：

1、 webpack   ./src/one.js   指定入口文件。将src/one.js打包到dist中的main.js里

2、webpack   ./src/one.js    --output   ../dist/one.js 指定入口文件和出口文件， 将你的src/one.js ---->上一级dist/main.js

3、打包多个文件，可以将其他的文件用import引入到一个文件中在进行打包

或者挨个写：webpack   src/one.js   src/two.js    --output    my/lala.js

4、加一个模式，

webpack   src/one.js   src/two.js    --output    my/lala.js   --mode   development

************************************

#### 进行配置

##### 1、package.json

```
{
  "name": "module2",
  "version": "0.0.1",
  "dependencies": {

  },
  "scripts":{
      "build":"webpack src/one.js src/two.js --output my/lala.js --mode production"
  }
}

npm run build 相当于 webpack src/one.js src/two.js --output my/lala.js --mode production
```



##### 2、webpack配置文件

```
{
    module,// 模块  loader
    entry,// 入口// 字符串，对象，数组
    output,// 出口
    devServer,// 开发环境配置 ，需要依赖plugins插件
    plugins，// 插件。
    mode:模式
}
```



###### 1、单入口单出口

```
const path = require("path");
module.exports = {
    mode:"development",
    entry:"./src/my.js",
    output:{
        path:path.resolve(__dirname,"../"),
        filename:"haha.js"
    }
}
```



###### 2、多入口单出口

```
module.exports = {
    mode:"development",
    entry:["./src/one.js","./src/two.js"],
    output:{
        filename:"bundle.js"
    }
}
```



###### 3、多入口多出口

```
module.exports = {
    mode:"development",
    entry:{
        one:"./src/one.js",
        two:"./src/two.js"
    },
    output:{
        filename:"[name].bundle.js"
    }
}
```



**************************************************************
##### webpack配置文件(webpack.config.js)

更改webpack配置文件的默认文件：
    webpack --config zhang.config.js :将默认配置文件，修改为zhang.config.js

**************************************************************

名字相同重复打包是覆盖

#### clean-webpack-plugin:

作用：将之前打包的内容清空，然后将最新打包内容放置到指定位置

1、下载

​	cnpm install clean-webpack-plugin  -D

2、引入   导入导出两种模式commentJS    ES6

 	const CleanWebpackPlugin  =  require("clean-webpack-plugin")

```
const {CleanWebpackPlugin} =  require("clean-webpack-plugin");
module.exports = {
    mode:"development",
    entry:{
        one:"./src/one.js",
        two:"./src/two.js"
    },
    output:{
        filename:"[name].bundle.js"
    },
	plugins:[  	//清空之前打包的内容清空
        new CleanWebpackPlugin()
    ]
}
```

*************************************

#### html-webpack-plugin

1、下载

​	cnpm   i   html-webpack-plugin   -D

2、引入

​	const   HtmlWebpackPlugin   =   require("html-webpack-plugin")

3、使用

```
const {CleanWebpackPlugin} =  require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin")
module.exports ={
    mode:"development",
    entry:{
        one:"./src/one.js",
        two:"./src/two.js"
    },
    output:{
        path:__dirname+"/dist",
        filename:"[name].aaa.js"
    },
   plugins:[
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            favicon:"./src/assets/img/my.gif",//将此图片作为站标
            template:"./public/index.html",//将index.html作为模板，用于决定生成的html文件
            filename:"home.html",  //将生成一个名字为home.html的文件，如果省略则默认为index.html
            // inject:false, //是否将生成的js文件嵌入到html当中，默认为true
            // inject:"body",//将script放到body中
            // inject:"head",//将script放到head中
            // hash:true, //是否在追加的js引入的地址当中增加一个后缀字符串
            title:"网易",//设置打包后的文件title标签的内容但是在设置之前需要先将模板中的title标签修改为<title><%=htmlWebpackPlugin.options.title%></title>
            arr:[1,2,3,4,5,6,7],//可以将数据在模板中用百度模板的方法进行遍历然后在打包文件中显示
            minify:{
                //对Html进行压缩
                removeComments:true,//打包后移除掉注释
                removeAttributeQuotes:true,//移除属性值的引号
                collapseWhitespace:true,// 折叠去除掉空格
            }
        })
    ],

}
```

********************************************************************************************************************

是否自动打开浏览器

```
devServer:{
    open:true,  //是否自动打开浏览器
    port:80,
    host:"127.0.0.1",
    proxy:{   //也可以代理
        "/m":{
            target:"http://m.maoyan.com",
            changeOrigin:true,
            pathRewrite:{
            	"^/m":""
            }
        }
    }
},
```

***************************************

开发环境服务器配置

1、下载

​	cnpm   i   webpack-dev-server   -g

​	cnpm   i   webpack-dev-server   -D

```
    module:{
        rules:[
            {
                //下载cnpm i style-loader css-loader -D
                //可以支持css
                //style-loader和css-loader的顺序不能换
                test:/.\.css$/,
                loader:["style-loader","css-loader"]
            },
            {
                //cnpm i sass-loader node-sass -D
                //可以支持sass
                test:/.\.scss/,
                loader:["style-loader","css-loader","sass-loader"]
            },
            {
                //cnpm i less-loader  less -D
                test:/.\.less/,
                loader:["style-loader","css-loader","less-loader"]
            },
            {
                //cnpm install url-loader file-loader -D
                text:/.\.(png|jps|gif)$/,
                loader:"url-loader",
                query:{
                    limit:0, //当图片大于Limit时，不转换为base64.使用的是文件地址 
                    outputPath:"img",//指定输出的文件夹
                }
                
                或者
                这样可以写多种
                text:/.\.(png|jps|gif)$/,
                use:[
                	{
                		loader:"usrl-loader",
                		query:{
                			limit,
                			outputPath
                		}
                	},{
                		loader:"usrl-loader",
                		query:{}
                	}
                ]
            }
        ]
    }

```

