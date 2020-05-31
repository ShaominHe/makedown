### react-redux

react-redux: 是基于redux，提升开发效率。当redux状态发生变化时，会执行render

#### 使用：

1、下载

​	cnpm  install  react-redux  -S

2、引入

```
import  {
	Provider,     //提供者，Provider组件可以通过自身的store属性向下传递store
				//只有被该组件包裹的组件才能获得store
	connect       //桥梁，用于连接组件的属性与store
				//是一个函数，高阶组件---将组件作为参数传给函数，再返回一个组件
}  from  "react-redux"
```

例子：

```
import React from 'react';
import ReactDOM from 'react-dom';
import store from "./store"
import App from './App';
import {
    Provider
} from "react-redux"

ReactDOM.render(<Provider store={store}><App></App></Provider>, document.getElementById('root'));
```



```
import React from "react"
import {
    connect     //函数，高阶组件
} from "react-redux"
class App extends React.Component {
    render(){
        return (
            <div>
                <input type="button" value={this.props.voteNum} onClick={this.props.changeVoteNum.bind(this)}/>
            </div>
        )
    }
        componentDidMount(){
            console.log(111,this.props)
        }
    }
    function mapStateToProps(state) {
        //1、返回的内容会被添加到组件当中的props对象当中
        //2、该函数接受的第一个参数即是store当中的状态state
        console.log(222,state)
        return {
            voteNum:state.voteNum
        }
    }
    function mapDispatchToProps(dispatch){
        return {
            changeVoteNum(){
                dispatch({
                    type:"CHANGE_VOTE_NUM"
                })
            }
        }
    }
    export default connect(mapStateToProps,mapDispatchToProps)(App)
//第一个函数是负责将state映射到属性当中
//第二个函数是负责将dispatch映射到属性当中
//函数返回的内容要是一个对象，他会将该对象的属性添加到组件的props当中
// export default connect(()=>{},()=>{})(App)
```



#### dangerouslySetInnerHTML  

react中将html字符串渲染到页面

#### 流程：

1、下载

2、使用Provider

3、创建组件连接connect

#### 异步action   使用thunk

同步action是一个对象，必须要有type属性，主要用途是操作数据状态

异步action是一个函数，在该函数体内可以包含异步操作，如果要更改数据状态，可以通过同步action

实现流程：

1、下载redux-thunk:

​			cnpm  i   redux-thunk  -S

2、使用

```
import {
	createStore,
	combineReducers,
	applyMiddleware    //是一个方法，用于添加你要使用的	     redux-thunk
} from "redux"
import thunk  from "redux-thunk"
```

创建一个仓库store

```
createStore(rootReducer,applyMiddleware(thunk))
```

#### bindActionCreators

作用：绑定上了actionCreator之后，在组件当中可以通过属性直接调用   actionCreator提供的异步action

```
import {
  bindActionCreators
} from "redux"

function mapDispatchToProps(dispatch){
  return bindActionCreators(newsActionCreate,dispatch)
}
```

#### 包裹标签：

1、<>  </>   不需要传递属性时，可以用空标签

2、Fragment

```
import React,{Fragment} from "react"
<Fragment 可以传递属性>
</Fragment>
```

#### 高阶组件：

是一个函数，将一个组件作为函数的参数，该函数返回一个组件。

1、withRouter

2、connect

作用：

1、属性代理：将某些共同的属性可以放置到高阶组件当中

2、反向继承：高阶组件当中继承传递过来的组件

constructor里的super可以调用父级的方法

```
 export function wrap(MyCom){
     class App extends React.Component{
         render(){
             return (
                 <div>
                     我是高阶组件
                     <MyCom userName={"laozhang"}></MyCom>
                 </div>
             )
         }
     }
     return App
 }
```

connect原理

```
 export function connect(stateToProps,dispatchToProps){
     return function(MyDom){
         class App extends React.Component{
             render(){
                 return(
                     <div>
                         <MyDom {...{
                             	...stateToProps({a:100}),
                           		...dispatchToProps()
                        }}></MyDom>
                     </div>
                 )
             }
         }
         return App
     }
 }
```

例子：做一个加载中的高阶组件

```
Loading  文件
import React from "react"
export default function Loading(){
    return <h3>拼命加载中...</h3>
}

import Loading from "./Loading"
export default function wrapLoading(MyCom){
    class App extends MyCom{
        render(){
            return this.state.isLoading?<Loading/>:super.render()
        }
    }
    return App
}
```



#### 发布与订阅：

解决传值问题

发布：publish

订阅：subscribe

**1、下载**

cnpm   install   pubsub-js   -S

**2、引入**

import   pubsub   from   "pubsub-js"

**3、使用**

订阅：

pubsub.subscribe（"消息的名字",函数）

相当于：

pubsub.subscribe（"消息的名字",(name,v)=>{}）

发布消息

pubsub.publish("消息的名字","传递的参数")     //异步

相当于：

pubsub.publishSync("消息的名字","传递的参数")     //异步

只执行一次：

pubsub.subscribeOnce（"消息的名字",(name,v)=>{}）  //只会执行一次

卸载指定的订阅：

pubsub.unsubscribe（"消息的名字")

清除所有的订阅：

pubsub.clearAllSubscriptions()       //清除所有的订阅者

#### 补充知识点：

写一个组件

```
import React from "react"
class MyCom extends React.Component{
    componentDidMount(){
        console.log(111,this.props)   //可以在这里接受
    }
    render(){
        return (
            <div>
                MyCom
                {this.props.children}   //可以将父级写入的值输出
              //{this.props.childrenp[1]}   //父级也可以写入多个，接受到的是数组
            </div>
        )
    }
}
export default MyCom
```

在App中引入并在组件中写入jsx

```
import React from 'react';
import './App.css';
import MyCom from "./components/MyCom"
class App extends React.Component{
  render(){
    return (
      <div className="App">
        <MyCom>
          <div>123456</div>   		//写入jsx	
          //<div>123456</div>			//也可以写多个，会组成数组传过去
        </MyCom>
      </div>
    )
  }
}
export default App
```

