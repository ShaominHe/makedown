### SASS

css清除浮动
![1568690192225](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1568690192225.png)

#### 简介

css编写时权重难以控制，嵌套层级多导致代码编写不方便，sass解决了这个问题
sass不能被浏览器直接识别，所以需要进行编译成正常的css文件才能被浏览器使用
sass的后缀有两种：scss、sass
后缀sass编写时没有大括号和分号，所以一般使用scss

#### 初体验

html结构

```
<div id="wrap">
    <div class="content">
        <div class="article">
            <p>
                <span>文字</span>
            </p>
        </div>
    </div>
</div>
```

css代码

```
#wrap .content{}
#wrap .content .article{}
#wrap .content .article p{}
#wrap .content .article p span{}
```

sass编写

```php
#wrap{
    width: 300px;
    height: 300px;
    border:3px solid #ccc;
    .content{
        width: 250px;
        height: 250px;
        background-color: pink;
        .article{
            width: 200px;
            height: 200px;
            background-color: skyblue;
            p{
                width: 150px;
                height: 150px;
                border:5px solid purple;
                span{
                    font-size: 20px;
                } 
            }
        }
    } 
}
```



#### sass的编译

使用sass之前，需要先安装，为了使用方便，直接全局安装

```
npm  i  -g  sass
```

编译sass成css

```
sass  待编译的sass文件路径  编译后的css文件路径
```

每一次修改sass文件就需要重新编译 一次，所以，node提供了一种实时编译

```
sass  --watch  待编译的sass文件路径	  编译后的css文件路径
```

但是这个只能监控一个文件，如果同时修改的文件较多则需要监控整个文件夹

```
sass  --watch  待编译的sass文件夹:编译后的css文件夹
```



#### sass的注释

```
//  单行注释，但是在编译的时候不保留
/*
	多行注释，编译的时候可以保留，压缩的时候不保留
*/
/*!
	多行注释，编译和压缩的时候都会保留
*/
```



#### sass的变量

在sass中用`$`来定义变量

可以定义一个统一的变量下面进行使用----如：颜色，像素

```php
$color:red;
$font_size:12px;
.header{
    background-color: $color;
    font-size:$font_size*2;
}
```



#### 嵌套

```scss
/*后代关系*/
.wrap{
	div{
		width:$font_size*10
	}
}
/*子类关系*/
ul{
	>li{
		padding:12px;
	}
}
/*大括号中表示自己---&写在哪个大括号中就代表哪个元素*/
.nav{
	&:hover{
		background-color:$color;
	}
	li{
		&:hover{
			color:$color;
		}
	}
}
/*群组嵌套按正常写即可*/
/*下一个兄弟元素*/
.login{
    color:green;
    &+div{	//选中了login的兄弟div
        width:100px;
    }
}
/*组合选择器*/
.left,.right{
    width:100px;
}
```



#### 属性嵌套

```
.box{
	border:1px solid #000{
		left:none;
		bottom-width:3px;
	}
}
```

编译后

```
.box{
	border:1px solid #000;
	border-left:none;
	border-bottom-width:3px;
}
```



#### 混合器

混合器可以理解为一个函数

```scss
/*定义混合器*/
@mixin	bor{
	border:1px solid red;
}
/*使用混合器*/
.box{
	@include bor;
}
带参数的混合器
@mixin bac($path,$color,$repeat){
	 background:url($path) $color $repeat;
}
/* 使用混合器 */
.box1{
    @include bac("img/1.jpg",red,no-repeat);
}
/* 带有默认值的参数 */
@mixin bac($path:"img/1.jpg",$color:blue,$repeat:no-repeat){
    background:url($path) $color $repeat;
}
/* 使用混合器 */
.box2{
    @include bac("img/2.jpg",green);
}
```



#### 继承

当下面的选择器需要使用到上面选择器的样式，就可以使用继承将上面的拿下来使用。

```
.tmp{
	width:100px;
	height:100px;
}
.box{
	@extend .tmp;
	background-color:red
}
```

编译后

```
.tmp,.box{
    width:100px;
    height:100px;
}
.box{
	background-color:red;
}
```



#### 导入

在一个文件中定义变量和混合器，在写css的时候文件比较混乱，所以通常会将变量和混合器放在单独的文件中，通过命令导入进来，这样每个文件中的代码都是同一类

变量文件

```
$red:red;
$size:100px;
```

混合器文件

```
@mixin bor($width,$color,$style:solid){
	border:$wdith $color $style
}
```

样式文件

```
@import "./variable.scss";
@import "./mixin.scss";

.box{
    @include bor(solid,1px,$red);
}
```

编译后

```
.box {
  border: solid 1px red;
}
```

导出：@export