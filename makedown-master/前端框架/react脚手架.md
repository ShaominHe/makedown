## react脚手架

### 脚手架的使用

1、安装
cnpm    install    create-react-app    -g

2、查看版本
create-react-app    -V
3、创建项目
create-react-app    项目名字
4、启动
cnpm start   启动项目

### 路由

1、如何跳转（通过标签）
2、编程式导航（通过程序）
3、如何接受参数
4、如何渲染组件

#### 路由的使用

1、下载
		cnom  install	react-router-dom    -S
2、引入
		import    {

​					NavLink,			链接
​					Link,				链接
​					Route,				路由
​					BrowserRouter    as    Router,      路由器，as是用来重命名
​					HashRouter    as    Router				哈希形式的路由器，效果是：多加一个#号，as是用来重命名
​					Switch,				对路由自上而下进行匹配
​					Redirect  			重定向

​		}    frrom    "react-router-dom"

***************************************************************************************************

##### Route和Router的区别：

Router:  路由容器
Route:  路由（具体的某个路由）

Router:
		basename:为路由统一设置前缀
		forceRefresh：强制刷新，每次请求均会重新创建一个实例。每次都会请求一次服务器

*******************************************************************************

##### 1、Navlink:可以实现路由的切换（导航）

​		exact:实现精确匹配。exact的值是一个布尔。默认为true
​		activeClassName:选中以后使用的样式
​		activeStyle:选中以后使用的样式
​		to:跳转到哪一个路由
***NavLink组件必须放到Router当中 （只要被Router包裹即可）

```
<Router>
    <nav>
        <NavLink className={"App-link"} exact={false} activeClassName={"App-active"} to={"/"}>首页</NavLink>|
        <NavLink className={"App-link"} activeClassName={"App-active"} to={"/search"}>搜索</NavLink>|
        <NavLink className={"App-link"} activeClassName={"App-active"} to={"/my"}>我的</NavLink>
    </nav>
</Router>

或者

<Router>
	<nav>
        <NavLink className={"App-link"} activeStyle={{color:"red"}} to={"/"}>首页</NavLink>|
        <NavLink className={"App-link"} activeStyle={{color:"red"}} to={"/search"}>搜索</NavLink>|
        <NavLink className={"App-link"} activeStyle={{color:"red"}} to={"/my"}>我的</NavLink>
	</nav>
</Router>
```

##### 2、Route必须被Router所包裹

​		1、exact:是否精确匹配
​		2、component:
​				1、外部引入组件，然后指定组件
​				2、直接赋值，箭头函数
​		path:如果当前路由的地址与path相同，则使用该路由下的组件
​	如果多个Route当中的path相同，均会使用
​		3、render:
可以给自定义的组件传值
传组件的话，组件会由render指定的组件渲染出来如:GuardRouter,可以实现拦截

```
<Route exact path={"/"} render={()=><GuardRouter userName={"老王"}></GuardRouter>}></Route>

也可以传组件
 <Route exact path={"/"} render={()=><GuardRouter component={Goods}></GuardRouter>}></Route>
```

接受

```
componentDidMount(){
    console.log(this.props)
}
```

Route

```
<Router>
     <nav>
         <NavLink className={"App-link"} exact={true} activeStyle={{color:"red"}} to={"/"}>首页</NavLink>|
         <NavLink className={"App-link"} activeStyle={{color:"red"}} to={"/search"}>搜索</NavLink>|
         <NavLink className={"App-link"} activeStyle={{color:"red"}} to={"/my"}>我的</NavLink>
     </nav>
		//箭头函数，直接赋值
     {/* <Route path={"/"} component={()=><div>我是首页</div>}></Route> */}
     //引入外部组件，指定组件
     <Route path={"/"} exact component={Home}></Route>
     <Route path={"/search"} component={Search}></Route>
     <Route path={"/my"} component={My}></Route>
</Router>
```

###### 404:

​		1、省略path
​		2、将path设置为*

```
<Route component={()=><div>404，您找的页面不存在</div>}></Route>
<Route path={"*"} component={()=><div>404，您找的页面不存在</div>}></Route>
```

***********************************************

##### 3、Switch:

Switch会将Route进行包裹，自上而下，只让第一个匹配路由的Route生效

```
<Router>
    <nav>
        <NavLink className={"App-link"} exact={true} activeStyle={{color:"red"}} to={"/"}>首页</NavLink>|
        <NavLink className={"App-link"} activeStyle={{color:"red"}} to={"/search"}>搜索</NavLink>|
        <NavLink className={"App-link"} activeStyle={{color:"red"}} to={"/my"}>我的</NavLink>
    </nav>

    {/* {只有符合条件的第一个会执行} */}
    <Switch>
        {/* <Route path={"/"} component={()=><div>我是首页</div>}></Route> */}
        <Route path={"/"} exact component={Home}></Route>
        <Route path={"/search"} component={Search}></Route>
        <Route path={"/my"} component={My}></Route>
        <Route path={"/my"} component={()=><div>我是下面的My</div>}></Route>
        {/* <Route component={()=><div>404，您找的页面不存在</div>}></Route> */}
        <Route path={"*"} component={()=><div>404，您找的页面不存在</div>}></Route>
    </Switch>   
</Router>
```

****************************

##### 4、Redirect：

路由的重定向，当from指向的地址满足条件时，跳转到to指向的地址

​		to:要重定向的地址
​		from:条件

```
<Router>
    <nav>
        <NavLink className={"App-link"} exact={true} activeStyle={{color:"red"}} to={"/"}>首页</NavLink>|
        <NavLink className={"App-link"} activeStyle={{color:"red"}} to={"/search"}>搜索</NavLink>|
        <NavLink className={"App-link"} activeStyle={{color:"red"}} to={"/my"}>我的</NavLink>
    </nav>

    {/* {只有符合条件的第一个会执行} */}
    <Switch>
        {/* <Route path={"/"} component={()=><div>我是首页</div>}></Route> */}
        <Route path={"/"} exact component={Home}></Route>
        <Route path={"/search"} component={Search}></Route>
        <Route path={"/my"} component={My}></Route>
        <Route path={"/my"} component={()=><div>我是下面的My</div>}></Route>
        {/* {当地址栏输入lalalala时会跳转到search页面} */}
        <Redirect to={"/search"} from={"/lalalala"}></Redirect>
        {/* <Route component={()=><div>404，您找的页面不存在</div>}></Route> */}
        <Route path={"*"} component={()=><div>404，您找的页面不存在</div>}></Route>
    </Switch>   
</Router>
```

*************************************************

##### 5、Link:

链接。没有activeStyle    activeClassName    这两个属性
Link适合用于链接。NavLink适合于导航

```
<Router>
    <nav>
        <Link className={"App-link"} exact={true} activeStyle={{color:"red"}} to={"/"}>首页</Link>|
        <Link className={"App-link"} activeStyle={{color:"red"}} to={"/search"}>搜索</Link>|
        <Link className={"App-link"} activeStyle={{color:"red"}} to={"/my"}>我的</Link>
    </nav>
</Router>
```

********************************************************************************************

##### 6、withRouter:

函数--------------》高阶组件：通过该高阶组件可以将路由属性向下传递
1、路由组件当中是有this.props.history/location/match
2、路由组件当中使用的组件是没有this.props.history/location/match

解决费路由组件无法获得    路由属性   方法

​	1、向下传递

```
<JoinCarBtn {...this.props}></JoinCarBtn>
```

​	2、通过withRouter高阶组件

```
import {withRouter} from "react-router-dom"
```

​	高阶组件：是一个函数，调用该函数时需要传递一个组件，该函数会返回一个组件：
​			1、属性代理
​			2、反向继承



******************************************************

##### to:

1、字符串

​			to={"/search"}

2、对象

{

​	pathname:"/xixi"

}

****************************************************

### <span style="color:red">传值三种</span>

#### 1、设置路由

​		1、如何传
​				1、设置路由		

```
<Route path={"/search/:id/:type"} component={Search}></Route>
```

​				2、跳转

```
<NavLink className={"App-link"} activeClassName={"App-active"} to={{pathname:"/search/1/2"}}>搜索</NavLink>
```

```
<NavLink className={"App-link"} activeClassName={"App-active"} to={"/search/1/2"}>搜索</NavLink>
```

​		2、如何接受

```
componentDidMount(){
	console.log(this.props.match.params.id)
	console.log(this.props.match.params.type)
}
```

刷新页面数据还在

#### 2、query

​		1、如何传

```
<NavLink className={"App-link"} activeClassName={"App-active"} to={{pathname:"/my",query:{id:10,type:100}}}>我的</NavLink>
```

​		2、如何接收

```
componentDidMount(){
    if(this.props.location.query){   //不判断会报错
        console.log(this.props.location.query.id)
        console.log(this.props.location.query.type)
    }
}
```

刷新数据不存在

#### 3、state

​		1、如何传

```
 <NavLink className={"App-link"} activeClassName={"App-active"} to={{pathname:"/goods",state:{id:11,type:111}}}>商品</NavLink>
```

​		2、如何接收

```
 componentDidMount(){
    console.log(this.props.location.state.id)
    console.log(this.props.location.state.type)
}
```

刷新数据还在。模拟表单提交，但是重新打开该网址时，数据丢失

*****************************************************************************

************************************************************************************************************************************************************************

### 二、编程式导航

```
render(){
    return (
        <div>
            <input type="button" value={"跳转首页"} onClick={()=>this.props.history.push("/")}/>
            <input type="button" value={"跳转搜索"} onClick={()=>this.props.history.push("/search/1/2")}/>
            <input type="button" value={"跳转我的"} onClick={()=>this.props.history.push({pathname:"/my",query:{id:12,type:14}})}/>
            <input type="button" value={"返回"} onClick={()=>this.props.history.go(-1)}/>
        </div>
    )
}
```

### 解决跨域：

第一种方法：

1、下载模块
		cnpm install  http-proxy-middleware  -D
2、在src目录下创建一个setupProxy.js文件
3、在setupProxy引入   http-proxy-middleware
4、配置

```
const proxy = require("http-proxy-middleware")
module.exports = function (app) {
    app.use("/abc",proxy({
        target:"http://127.0.0.1",
        changeOrigin:true,
        pathRewrite:{
            "^/abc":""
        }
    }))
}
```

第二种方法：

在package.json中写一个

```
"proxy":"http://127.0.0.1"
```

将页面上的请求地址中的http://127.0.0.1去掉

修改完之后，重启

### react加载本地图片

1、通过import引入

```
import yesPic from "../../assets/images/yes.png"
import noPic from "../../assets/images/no.png"
```

2、通过require导入

```
 <img src={require("../../assets/images/yes.png")} alt=""/>
```

直接写img标签src属性加载不出来