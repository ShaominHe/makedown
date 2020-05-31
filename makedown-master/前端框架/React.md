### React

#### class

```
class Box{
    //constructor方法相当于之前构造函数，方法外的内容全部都在原型对象当中
    constructor(){
        this.userName="张三";
        this.age=12;
    }
    run(){
        console.log(this.userName+"今年"+this.age)
    }
}
// Box.prototype.userName="李四"
const obj=new Box();
// obj.__proto__.run()   //李四
// obj.__proto__.run.call(obj)   //张三
// obj.__proto__.run.bind(obj)()   //张三
// obj.run()

//相当于
// function Box(){
//     this.userName="张三";
//     this.age=12;
// }
// Box.prototype.run=function(){
//     console.log(this.userName+"今年"+this.age)
// }
```

class掌握如何定义、如何实例化、如何传值、如何继承

```
class StrongTag extends Tag {
    constructor(options){    //继承的固定写法，参数options可以随便写
        super(options)
    }
}
```



#### 面向对象特点：

封装、继承、多态、抽象

#### JSX

jsx:javascript xml

#### 元素变量

```
<script type="text/babel">
//元素变量：值为元素的变量称为元素变量，元素首字母必须小写如h1的h必须小写，因为大写是组件
    const str=<h1>孤</h1>  //h必须是小写
    ReactDOM.render(str,document.querySelector("#myApp"))
</script>
```

#### 覆盖

```
<script type="text/babel">
//多个render挂载相同的元素，以最后一个为准
    ReactDOM.render("张三",document.querySelector("#myApp"))
    ReactDOM.render("李四",document.querySelector("#myApp"))
    //结果是李四
</script>
```

#### 建议

```
<script type="text/babel">
//建议第一个元素，要呈现的内容放在括号里
    ReactDOM.render((
        <h1>建议加括号使用</h1>
    ),document.querySelector("#myApp"))
</script>
```

注意：

<span style='color:red'>jsx有且只能有一个根元素</span>

#### 遍历数组

```
<script type="text/babel">
    //react可以直接展开你的数组，并且数据当中也可以支持jsx
    const arr=[<h2>h2</h2>,<div>div</div>,<p>p</p>,<i>i</i>]
    ReactDOM.render(arr,document.querySelector("#myApp"))
</script>
```

##### 遍历二维数组

```
<script type="text/babel">
    //单大括号为输出
    //写一些逻辑判断，js语句
    const arr=[[1],[2],[3]]
    ReactDOM.render((
    <div>
        {
            arr.map(v=>(
                v.map(num=>(
                    <h1 key={num}>{num}</h1>
                ))
            ))
        }
    </div>
    ),document.querySelector("#myApp"))
</script>
```



#### 引入外部

```
如果引入外部文件，使用的是jsx，需要增加type属性
<script src="lib/react-jsx.js" type="text/babel"></script>
```

#### 输出

1、单大括号为输出

```
<script type="text/babel">
    //单大括号为输出
    const num=1
    ReactDOM.render((
    <div>
        {num}
    </div>
    ),document.querySelector("#myApp"))
</script>
```

2、输出中也可以写逻辑判断，js语句

```
<script type="text/babel">
    //单大括号为输出
    //写一些逻辑判断，js语句
    const sex=1
    ReactDOM.render((
    <div>
        {sex===1?"男":"女"}
    </div>
    ),document.querySelector("#myApp"))
</script>
```

#### 遍历对象

```
<script type="text/babel">
    //对象不允许直接输出
    const obj=[
        {
            bookName:"天龙八部"
        },
        {
            bookName:"行尸走肉"
        }
    ]
    ReactDOM.render((
    <div>
        {
            obj.map((v,i)=>(
                <h1 key={i}>《{v.bookName}》</h1>
            ))
        }
    </div>
    ),document.querySelector("#myApp"))
</script>
```

#### jsx加类名

```
<style>
    .wrap{
        width:100px;
        height:100px;
        background-color: red;
    }
</style>
<body>
    <div id="myApp">

    </div>
</body>
<script type="text/babel">
    //加类名建议使用className
    ReactDOM.render((
        <div className="wrap">

        </div>
    ),document.querySelector("#myApp"))
</script>
```

#### jsx加style属性

```
<script type="text/babel">
    ReactDOM.render((
        <div style={{
            width:"100px",
            height:"100px",
            background:"red"
        }}>

        </div>
        ),document.querySelector("#myApp"))
</script>
```

#### 事件 

1、

```
<script type="text/babel">
    //事件使用驼峰命名法
    function fn(){
        alert(1)
    }
    const str="点击"
    ReactDOM.render((
        <div>
            <input type="button" onClick={fn} value={str}/>
        </div>
    ),document.querySelector("#myApp"))
</script>
```

2、直接将函数赋值给事件

```
<script type="text/babel">
    const str="点击"
    ReactDOM.render((
        <div>
            <input type="button" onClick={function(){
                alert(2)
            }} value={str}/>
        </div>
    ),document.querySelector("#myApp"))
</script>
```

或者写为箭头函数

```
<script type="text/babel">
    const str="点击"
    ReactDOM.render((
        <div>
            <input type="button" onClick={()=>{
                alert(3)
            }} value={str}/>
        </div>
    ),document.querySelector("#myApp"))
</script>
```

#### 事件传值

```
<script type="text/babel">
    //传值使用bind传，第一个参数是改变this指向，后面是传的参数
    //而函数中会接受一个事件对象
    function fn(a,b,e){
        console.log(a,b,e.target.value)
    }
    ReactDOM.render((
        <div>
            <input type="button" onClick={fn.bind(this,1,2)} value="点击"/>
        </div>
    ),document.querySelector("#myApp"))
</script>
```

#### 组件

组件是对标签的一种扩展，组件的首字母必须要大写

##### 无状态组件(function)

```
<script type="text/babel">
//定义一个名字为My的无状态组件，且必须有返回值，返回的元素只能有一个根元素，建议return的内容写在括号里
    function My(){
        // return (<div>123</div>)
        // return (123)
        return (null)  //没有返回具体的内容
    }
    ReactDOM.render((
        <My></My>
    ),document.querySelector("#myApp"))
</script>
```

向下传值

```
<script type="text/babel">
//向下传值语法：
    // {...v}  将你的对象进行展开并向下合并
    const siteInfo=[
        {
            siteName:"腾讯",
            siteUrl:"http://qq.com"
        },
        {
            siteName:"狗狗",
            siteUrl:"http://gougou.com"
        }
    ]
    function Site(props){
        return (
            <div>
                <h1>站点名称：{props.siteName}</h1>
                <h4>站点网址：{props.siteUrl}</h4>
            </div>
        )
    }
    ReactDOM.render((
        <div>
            {   //是下面方法的简写，...v可以将v展开
                siteInfo.map((v,i)=>(
                    <Site key={i} {...v}></Site>
                ))
                //写为组件
                // siteInfo.map((v,i)=>(
                //     <Site key={i} siteName={v.siteName} siteUrl={v.siteUrl}></Site>
                // ))
                //正常写法
                // siteInfo.map((v,i)=>(
                //     <div key={i}>
                //         <h1>站点名称：{v.siteName}</h1>
                //         <h4>站点网址：{v.siteUrl}</h4>
                //     </div>
                    
                // ))
            }
        </div>
    ),document.querySelector("#myApp"))
</script>
```



##### 状态组件(class)

```
<script type="text/babel">
//状态组件需要class定义.首字母必须要大写，必须要有返回值（状态组件的返回值要写在render当中）

//定义了一个名字为My的状态组件
    class My extends React.Component {
        constructor(){  //当constructor中有自己额外定义的属性时，不可以省略
            super();
        }
        render(){
            return (
                <div>
                    123123
                </div>
            )
        }
    }
    ReactDOM.render((
        <div>
            <My></My>
        </div>
    ),document.querySelector("#myApp"))
</script>
```

状态组件传值

```
<script type="text/babel">
    //属性时不允许被修改的，状态是允许被修改的
    class One extends React.Component {
        //在constructor中定义状态
        //修改，通过this.setState进行修改
        constructor(){
            super();
            this.state={
                age:12
            }
        }
        render(){
            return (
                <div>One
                console.log(111)
                <input type="button" onClick={()=>{ 
                    console.log(111,this.state.age)
                    //setState是异步
                    this.setState({    //将数据修改，将页面在重新执行一次
                        age:this.state.age+1
                    },function(){
                        //当数据更新完毕以后执行。
                        console.log(222,this.state.age)
                    })
                    // this.state.age++
                    console.log(333,this.state.age)
                    }} 
                    //111和333都是比结果慢一步，因为setState是异步
                    value={this.state.age}
                />
                    <Two age={this.state.age}></Two>
                </div>
            )
        }
    }
    class Two extends React.Component {
        render(){
            return (
                <div>Two{this.props.age}</div>
            )
        }
    }
    ReactDOM.render((
        <One></One>
    ),document.querySelector("#myApp"))
</script>
```

状态组件

1、如何定义

```
constructor(){
	super();
	this.state={
		//定义数据状态
	}
}
```

2、修改状态

​	1、直接修改
​	this.state.num=2    //通过该方法视图不会更新

​	2、通过setState进行修改（异步），会重新执行你的render

​	this.setState({

​			num:2

​	},function(){	

​			console.log("该回调函数会在数据更新完毕以后调用")

​	})

###### 状态组件解决this

状态组件当中的箭头函数，解决this问题的四种方法
1、直接赋予箭头函数
	缺点：当你的函数体内的逻辑比较复杂，或代码量较大时，不适宜用该方法
2、通过bind
	缺点：当组件内使用该方法频率较多时，都要增加bind
3、在构造器当中，为其进行绑定
	缺点：传值比较麻烦
4、直接赋值通过箭头函数
	changeNum=()=>{

​				this.setState({

​						num:++this.state.num

​				})

​	}

##### 受控组件与非受控组件

受控组件：组件当中的值受state的控制，需要在使用时增加onChange方法   value绑定上state的值

非受控组件：通过ref  defaultValue   defaultChecked

###### 受控组件

```
<script type="text/babel">
    class App extends React.Component {
        constructor(){
            super(),
            this.state={
                userName:"",
                passWord:""
            }
        }
        handlerChange(e){
            console.log(e.target.name)
            this.setState({
                [e.target.name]:e.target.value
            })
                // console.log(this.state.userName)
        }
        submitForm(){
            console.log(this.state)
        }
        render(){
            return (
                <div>
                    <input type="text" name={"userName"} onChange={this.handlerChange.bind(this)} value={this.state.userName}/>
                    <br/>
                    <input type="password" onChange={this.handlerChange.bind(this)} name={"passWord"} value={this.state.passWord}/>
                    <input type="button" onClick={this.submitForm.bind(this)} value="提交"/>
                </div>
            )
        }
    }
    ReactDOM.render((
        <App/>
    ),document.querySelector("#myApp"))
</script>
```

###### 非受控组件

1、ref="属性名"     this.refs.属性名

```
<script type="text/babel">
    class App extends React.Component {
        render(){
            return (
                <div>
                    <input type="text" ref={"userName"}/>
                    <input type="button" value={"提交"} onClick={()=>{
                        console.log(this.refs.userName.value)
                    }}/>    
                </div>
            )
        }
    }
    ReactDOM.render((
        <App/>
    ),document.querySelector("#myApp"))
</script>
```

相同的ref如果绑定了多个元素，只会得到最后一个

2、ref={()=>{}}
	1、箭头函数在挂载时会执行
	2、会接受一个参数，该参数即是ref所在的元素

```
<script type="text/babel">
    class App extends React.Component {
        constructor(){
            super()
            this.formdata={
                userName:"张三"
            }
        }
        aaa(){
            //可以获得数据
            console.log(111,this.formdata.userName.value)
        }
        render(){
                console.log(222,this.formdata.userName)
            return (
                <div>
                        <input type="text" defaultValue={this.formdata.userName} ref={v=>{this.formdata.userName=v}}/>
                        <input type="button" value="提交" onClick={this.aaa.bind(this)}/>
                </div>
            )
        }
    }
    ReactDOM.render((
        <App/>
    ),document.querySelector("#myApp"))
</script>
```

##### 静态方法     static

不需要对类进行实例化，既可以直接使用的方法称为静态方法

### react中的小插件

#### react-tooltip

react-tooltip是一个鼠标放上去可以显示悬浮的插件

1、install

npm i react-tooltip -S

2、import

import ReactTooltip from "react-tooltip"

3、use

在需要鼠标悬停显示的标签上加上data-tip和data-html属性

![Capture](C:\Users\shaomin.he\Desktop\Capture.PNG)

详情见： https://www.cnblogs.com/zizaiwuyou/p/10868410.html 

#### react-style-box

what is it?

 This is a simple [styled.div](https://github.com/styled-components/styled-components) component, that makes your basic positioning of elements faster. Instead of writing all of the "styled" components upfront you can prototype the layout faster with this ready 

 译：这是一个简单的style.div组件，它可以加快元素的基本定位。不需要预先编写所有的“样式化”组件，这样可以更快地创建布局原型 

详情见： https://www.npmjs.com/package/react-styled-box 

#### react-katex

display math expressions with KaTeX and React

译：用KaTeX和React显示数学表达式

1、install

npm i react-katex 

2、import

import 'katex/dist/katex.min.css'

import {InlinMath,BlockMath} from 'react-katex'

describe：

InLineMath：display math in the middle of the text

![1579360699958](C:\Users\shaomin.he\AppData\Roaming\Typora\typora-user-images\1579360699958.png)

rtm project

![1579360721269](C:\Users\shaomin.he\AppData\Roaming\Typora\typora-user-images\1579360721269.png)

BlockMath：display math in a separated block,with larger font and symbols

译：将数学显示在一个单独的块中，使用更大的字体和符号。

![1579360518527](C:\Users\shaomin.he\AppData\Roaming\Typora\typora-user-images\1579360518527.png)

详情见： https://www.npmjs.com/package/react-katex 

#### reactstrap

Adding Bootstrap

Install reactstrap and Bootstrap from NPM,Reactstrap does not include Bootstrap CSS so this needs to be installed as well

install：

npm install bootstrap -S

npm install reactstrap react react-dom -S

import:

import "bootstrap/dist/css/bootstrap.css"

需要使用什么组件就引入什么

for example:

import {Button,Collapse} from "reactstrap"

详情： https://reactstrap.github.io/ 

#### react-toastify

是一个弹出消息的插件，

install:

npm install --save react-toastify

import :

import {toast} from "react-toastify"

use:

![1579362992145](C:\Users\shaomin.he\AppData\Roaming\Typora\typora-user-images\1579362992145.png)

