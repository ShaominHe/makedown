### gulp

#### 引入

项目做好以后，在上线之前还有一些工作需要去做：
	压缩静态资源
	变更静态资源
	给静态资源添加md5
	修改预处理样式后自动编译（SASS,Less）
	合并雪碧图
	...	

等待，在前端工作流出现之前，这些工作都有人力完成，而这些工作往往比写业务本身更加费时，效率非常之低而且还容易出错，于是自动化工具就出现了

前端的构建工具常见的有Grunt、Gulp、Webpack三种，Grunt比较老旧，功能少，更新少，插件少

#### 概念

gulp是一个自动化构建工具，主要用来设定程序自动处理静态资源的工作。简单的说，gulp就是用来打包项目的。

官网：https://gulpjs.com/

中文官网：https://www.gulpjs.com.cn/docs/

#### 安装

先全局安装

```
npm i -g  gulp@3.9.1
gulp -v #检测是否安装成功
```

在进行初始化

```
npm init -y(使用英文文件夹，不能使用大写字母)
```

在局部安装

```
npm i gulp@3.9.1  --save-dev #因为在上线后是不需要这个包的，所以讲这个项目安装在开发依赖
```

首先gulp和node中的其他模块是一样的，使用的时候需要引入

```
const gulp = require("gulp"); 
```

#### 使用

##### 注册任务

```
gulp.task(name[,deps],fn)
```

参数1：name是任务名称，执行任务时，使用这个名称
参数2：fn是一个回调函数，代表这个任务要做的事情

##### 执行任务

```
gulp  name
```

如果任务比较多的话，一个一个执行效率低，所以提供了一个默认任务，可以将要执行的所有任务放在一个数组中，这样只需要执行这个默认任务就能执行数组中的所有任务

```
gulp.task("print1",function(){
    console.log("打印123");
})
gulp.task("print3",function(){
    console.log("打印321");
})
gulp.task("default",["print1","print3"]);
```

执行默认任务，不用写任务名

```
gulp
```

##### 读取文件

```
gulp  src(globs)
```

参数：读取的目标的源文件的路径

##### 输出文件

```
gulp.dest(path)
```

参数：dest方法主要用来讲数据输出到文件中，所以参数就是目标文件路径

##### 监视文件变化

用来监视某个或某些文件发生变化，可以在变化的时候，执行一个回调函数，以保证文件中的代码和效果一样

```
gulp.watch()
```

例：

```
gulp.task("sass",function(){
	gulp.src("./scss/*.scss")
	.pipe(sass())
	.pipe(gulp.dest("./css/"))
})
gulp.task("watch",["default"]function(){
	gulp.watch("./sass/*.scss",["sass","mincss"]) //第一个参数表示监视的文件，第二个参数是文件发生变化后要执行的任务,监视任务应该依赖于默认任务,否则会让命令卡在那里
})
```



#### gulp插件

插件下载：

```
npm install 插件名 --save-dev
```

gulp-concat:合并文件(js/css)
gulp-uglify : 压缩js文件
gulp-rename : 文件重命名
gulp-less : 编译less
gulp-sass：编译sass
gulp-clean-css : 压缩css
gulp-livereload : 实时自动编译刷新
gulp-htmlmin：压缩html文件
gulp-connect：热加载，配置一个服务器
gulp-load-plugins：打包插件（里面包含了其他所有插件）

##### 合并压缩css

先下载：

```
npm i gulp@3.9.1 gulp-concat gulp-clean-css gulp-rename --save-dev
```

<font color=red>再建一个gulpfile,js的文件</font>

在这个文件中进行操作

```
const gulp=require("gulp");
const cleanCss=require("gulp-clean-css");
const concat=require("gulp-concat");
const rename=require("gulp-rename");

gulp.task("css",function(){
	gulp.src("./css/*.css") //将当前文件夹下所有后缀是.css的文件都读取到内存中
	.pipe(concat("index.css")) //合并css文件并放入指定的文件中
	.pipe(cleanCss({	//压缩css文件
		compatibility:'ie8'	//让样式兼容到ie8
	}))
	.pipe(rename({	//重命名
		suffix:".min"
	}))
	.pipe(gulp.dest("./min")) //将这个目标文件保存到一个文件夹中
})
```

补充：重命名：

rename({

​	prefix:"bonjour-"  //前缀

​	suffix:"-min"		//后缀点之前的位置

​	extname:".md"	//后缀

})



##### 合并压缩js

压缩js文件的时候如果函数没有使用，那么就会忽略这个函数

```
const gulp = require("gulp")
const rename = require("gulp-rename")
const uglify = require("gulp-uglify")
const concat= require("gulp-concat")

gulp.task("concatJs",function(){
	gulp.src(./js/*.js)
	.pipe(concat("result.js"))
	.pipe(uglify())
	.pipe(rename({
		suffix:".min"
	}))
	.pipe(gulp.dest("./"))
})
```



##### 解析sass语法

需要使用gulp-sass插件

```
gulp.task("sass",function(){
    gulp.src("./scss/test.scss")
    .pipe(sass())	//没有文件夹时会新建文件夹
    .pipe(rename({
        suffix:".min"
    }))
    .pipe(gulp.dest("./css"))
})
```

##### 压缩html

需要插件gulp-htmlmin插件

```
gulp.task("html",function(){
    gulp.src("./index.html")
    .pipe(htmlmin({
        collapseWhitespace:true  //必须加这个参数
    }))
    .pipe(rename({
        suffix:".min"
    }))
    .pipe(gulp.dest("./"))
})
```



##### 扩展

###### css自动添加代码前缀

目的：将一些不兼容的css属性添加前缀让各个浏览器兼容
依赖插件：gulp-autoprefixer

```
const gulp=require("gulp");
const autoprefixer=require("gulp-autoprefixer");

gulp.task("prefixCss",function(){
	gulp.src(./prefix.css)
	.pipe(autoprefixer({
		"browsers":["last 2 version","ios>7","Firefox>20"]
	}))
	.pipe(rename({
		prefix:"-pre-"
	}))
	.pipe(gulp.dest(./))
})
```

这时会出现一个提示，希望将这个配置写在package.json中：
此时需要在package.json中添加：

```
"browserslist":["last 2 version","ios>7","Firefox>20"]
```

并将原来文件中的browsers删掉

###### 清空目标文件夹

如果每次打包的时候起不一样的名字，会造成有些文件没有用，但是还占据空间。所以每次在打包之前应该先将之前的文件夹情空，然后再打包。

例：当编译完成后，再次修改内容再次编译会出现后缀后面加后缀

![1568818063497](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568818063497.png)

所以此时应该先清空css文件夹，需要用到glup-clean插件

```
gulp.task("clean",function(){
	gulp.src("./css")
	.pipe(clean())  //会直接将文件夹都删掉
})
```

补充：当将整个文件夹都删掉时，在此编译sass会新建文件夹

###### es6转es5

为了让更多浏览器兼容项目，需要将项目中的es6的语法转为es5的语法。

依赖的插件有：

```
gulp-babel@7.0.1
babel-core
babel-preset-es2015
```

导入的时候只要导入一个即可：

```
const babel = require('gulp-babel')
```

代码

```
gulp.task('es6toes5', function () {
    	gulp.src('./src/js/**')
    	.pipe(babel({
        	presets: ['es2015'] // 必须加这句话
    	}))
    	.pipe(gulp.dest('./'))
})
```

###### 所有的包

gulp提供了一个插件，可以代替所有插件，也就说只要下载这一个插件就可以使用其他所有插件了
下载插件：

```
npm install gulp-load-plugins --save-dev
```



#### 特殊任务

##### 默认任务

```
gulp.task("default",["任务1","任务2"...])
```

执行命令是

```
gulp
```



##### 同步异步任务

代码前加上return是异步，删掉return是同步，如果是依赖任务，这两个必须是异步任务

![1568865623346](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568865623346.png)

![1568865592676](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568865592676.png)



##### 依赖任务

压缩css时必须是解析完sass以后在执行，task这个方法，第二个参数应该是一个可选参数，是一个数组，里面放的是依赖任务名称,先执行依赖任务，等依赖任务执行完后在执行前面的第一个参数

```
gulp.task("css",["sass"],function(){
	gulp.src("css/*.css")
	.pipe(cleanCss())
	.pipe(rename{
		suffix:".min"
	})
	.pipe(gulp.dest("./css/"))
}])
```

#### 全自动构建项目

下载插件：

```
npm i gulp-connect --save-dev
```

根据插件启动一个服务器

```js
// 注册全自动监视任务
gulp.task("server",["default"],function(){
    // 配置服务器选项
    connect.server({
        root:'./dist/', // 配置服务器根目录
        livereload:true, // 实时刷新
        port:5000 // 配置服务器端口
    });
    // 确认监听的目标文件以及绑定相应的任务
    gulp.watch("./src/js/*.js",["handleJs"]);
    gulp.watch(["./src/css/*.css","./src/sass/*.scss"],["handleCss"]); // 监听多种文件放在数组中
});
```

还需要在每个任务中添加实时刷新设置：

```
.pipe(connect.reload()) // 将更改后的内容实时填充到页面中
```

也可以下载插件自动打开浏览器

```shell
npm i open --save-dev
```

例：

```js
var open = require("open");
// 注册全自动监视任务
gulp.task("server",["default"],function(){
    // 配置服务器选项
    connect.server({
        root:'./dist/', // 配置服务器根目录
        livereload:true, // 实时刷新
        port:5000 // 配置服务器端口
    });
    // 自动打开指定连接
    open("http://localhost:5000");
    // 确认监听的目标文件以及绑定相应的任务
    gulp.watch("./src/js/*.js",["handleJs"]);
    gulp.watch(["./src/css/*.css","./src/less/*.scss"],["handleCss"]); // 监听多种文件放在数组中
});
```

这时候只要我们启动这个任务，就会自动帮我们打开网页。





## 总结

使用gulp步骤：

1、下载全局gulp工具（指定版本）：npm i -g gulp@3.9.1

2、初始化：npm init -y（使用英文的文件夹，不能使用大写字母）

3、局部安装需要的包（gulp@3.9.1    gulp-clean-css    gulp-concat    gulp-rename    gulp-uglify   	gulp-babel(这需要下载babel-core  	babel-preset-es2015)	 gulp-autoprefixer 	gulp-sass 	gulp-scss 	gulp-less 	gulp-htmlmin 	gulp-conenct 	gulp-clean）

4、创建gulpfile.js文件



使用全自动构建项目：

1、注册任务

2、使用connect包创建服务器

3、传入参数：根目录、端口、自动刷新

4、给所有任务添加自动刷新：connect.reload()

5、自动打开网页：open(要打开的网页地址)