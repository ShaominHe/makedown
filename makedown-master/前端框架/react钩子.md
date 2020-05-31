### react生命周期

#### 生命周期

**挂载：mounting**      除了render之外，其他的钩子只会在生命周期当中出现一次

​	执行流程：
​				1、constructor    构造器
​				2、componentWillMount    挂载之前
​				3、render  	呈现并渲染
​				4、componentDidMount	挂载之后

**更新：updating**

1、属性

​		1、componentWillReceiveProps(nextProps):即将要更改的值
​		2、shouldComponentUpdate(nextProps):是否要更新，必须有一个返回值
​				true:
​						componentWillUpdate(nextProps):即将更新数据
​						render
​						componentDidUpdate(preProps):更新完毕

​				false:会到运行中

2、状态

​		shouldComponentUpdate(nextProps,nextState)
​				true:
​						componentWillUpdate(nextProps):即将更新数据
​						render
​						componentDidUpdate(prpProps):更新完毕
​				false:回到运行中

**卸载：unmounting**

componentWillUnmount

#### 钩子

只有状态组件里面才有钩子，非状态里面没有

##### 挂载：mounting

```
<script type="text/babel">
class App extends React.Component {
    constructor(){
        super()
        console.log("1、constructor")
    }
    render(){
        console.log("3、render")
        return (
            <div>123</div>
        )
    }
    componentWillMount(){
        //即将挂载
        console.log("2、componentWillMount")
    }
    componentDidMount(){
        //挂载结束
        console.log("4、componentDidMount")
    }
}
ReactDOM.render((
    <App/>
),document.querySelector("#myApp"))
</script>
```

##### 更新：updating

1、属性

```
<script type="text/babel">
    class Child extends React.Component {
        render(){
            console.log("4、render")
            return (
                <div>{this.props.age}</div>
            )
        }
        //nextProps:将要更改的值
        componentWillReceiveProps(nextProps){
            //程序判断属性与之前的属性是否相同，如果不同，可以写一些逻辑
            if(this.props.age!==nextProps.age){
                //可以写一些逻辑
            }
            console.log("1、componentWillReceiveProps",this.props.age,nextProps.age)  //1,2
        }
         //必须要有一个返回值，true代表执行render，false代表不执行render
        //接受两个参数，第一个是更改的属性，第二个是更改的状态
        shouldComponentUpdate(nextProps){
            console.log("2、shouldComponentUpdate",nextProps.age,this.props.age)   //2,1
            if(nextProps.age !== this.props.age){
                return true
            }else{
                return false
            }
        }
        //更新前
         componentWillUpdate(nextProps){
            console.log("3、componentWillUpdate",this.props.age,nextProps.age)  //1,2
        }
        //更新后
        //preProps是更新之前的值
        componentDidUpdate(preProps){
            console.log("5、componentDidUpdate",this.props.age,preProps.age)    //2,1
        }
    }
    class App extends React.Component {
        constructor(){
            super();
            this.state={
                age:1
            }
        }
        render(){
            return (
                <div>
                    <input type="button" value="修改" onClick={()=>{
                        this.setState({
                            age:this.state.age+1
                        })
                    }}/>
                    <Child age={this.state.age}></Child>
                </div>
            )
        }
    }
    ReactDOM.render((
        <App/>
    ),document.querySelector("#myApp"))
</script>
```

2、状态

```
<script type="text/babel">
    class App extends React.Component {
        constructor(){
            super();
            this.state={
                num:1
            }
        }
        render(){
            console.log("3、render")
            return (
                <div>
                    <input type="button" value={this.state.num} onClick={()=>{
                        this.setState({
                            num:this.state.num+1
                        })
                    }}/>
                </div>
            )
        }
        shouldComponentUpdate(nextProps,nextState){
            console.log("1、shouldComponentUpdate",this.state.num,nextState.num)   //1,2
            // if(nextState.num>5)
            //     return false;
            return true;
        }
        //即将更改
        componentWillUpdate(nextProps,nextState){
            console.log("2、componentWillUpdate",this.state.num,nextState.num)
        }
        //更改完毕
        componentDidUpdate(preProps,preState){
            console.log("4、comonentDidUpdate",this.state.num,preState.num)    //2,1
        }
    }
    ReactDOM.render((
        <App/>
    ),document.querySelector("#myApp"))
</script>
```



##### 卸载：unmounting

```
<script type="text/babel">
    class My extends React.Component {
        render(){
            return (
                <div style={{
                    width:"100px",
                    height:"100px",
                    background:"red"
                }}>12313</div>
            )   
        }
        componentWillUnmount(){
            console.log("结束了")
        }
    }
    class App extends React.Component {
        constructor(){
            super();
            this.state={
                isShow:true
            }
        }
        render (){
            return (
                <div>
                    <input type="button" value="修改" onClick={()=>{
                    this.setState({
                        isShow:!this.state.isShow
                    })
                    }}/>
                    {this.state.isShow?<My></My>:null}
                </div>
                
            )
        }
    }
    ReactDOM.render((
        <App/>
    ),document.querySelector("#myApp"))
</script>
```

