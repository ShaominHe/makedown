### redux

redux:实现数据集中式的管理

vuex只能在vue中使用，而redux所有的框架都能用

#### 组成

store:  仓库，存放的是你的state（数据）                                仓库

state:  数据。																				商品

reducer:  函数。通过该函数来进行数据进行操作					仓库的工作人员

action:  行为。用于决定对数据进行怎样的操作 			命令工作人员进行什么操作

dispatch:  执行action															执行命令让工作人员



*store当中存放的是state

*action唯一的执行正途是通过dispatch

*

*******************************************************************************************************************************************************************************

#### 使用：

1、下载

cnpm   install   redux   -S

2、引入

import    {createStore}    from    "redux"



##### 使用

###### 如何创建仓库

1、store与state的关系：  const state = store.getState()

 2、创建仓库是需要使用createStore方法，并将reducer函数作为参数进行传入

  			const store = createStore(reducer)

 3、reducer返回的内容即是你的状态值（state）

 4、reducer接受的参数有两个分别为state和action。

 5、状态不允许直接修改。只有通过dispatch方法执行action才是操作state的正途

6、dispatch是一个函数，需要给予该函数一个action，action必须是一个对象，对象里必须有一个type属性，type用于判断你的行为类别

7、建议action的type值采用大写

8、reducer的执行，在创建仓库的时候,reducer会执行，在执行dispatch时也会执行。

9、当你执行dispatch时，state是上一次你最终操作的状态

```
import {
    createStore
} from "redux";
/************创建reducer******************* */
const initState={    //也可以不写这个，直接给state的默认值赋值
    userName:"老李"
}
//state：当你执行dispatch时，state是上一次你最终操作的状态
const reducer = function(state=initState,action){
    state=JSON.parse(JSON.Stringify(state))
    if(action.type==="CHANGE_USER_NAME")
        state.userName="老王"
    return state
};
/************创建仓库******************* */
const store = createStore(reducer);
/************获得当前的状态******************* */
const state = store.getState();
/************通过dispatch执行你的action******************* */
store.dispatch({			//这个大括号里面的就是action
    type:"CHANGE_USER_NAME"
})
```

###### 深复制和浅复制：

深复制的话，改变一个另一个不变，

浅复制，改变一个都改变-------Array.from,  ...obj  ,   Object.assign

深复制的方法：

```
JSON.parse(JSON.String(state))
```

###### 将要修改的数据通过payload进行传递

```
import {
    createStore
} from "redux";
const initState={
    userName:"老李"
}
const reducer = function(state=initState,action){
    state=JSON.parse(JSON.stringify(state))
    if(action.type==="CHANGE_USER_NAME")
        // state.userName="老王"
        state.userName=action.payload.userName
    return state
};
const store = createStore(reducer);
const state = store.getState();
console.log(state)   //老李
store.dispatch({        //这个大括号里面的就是action
    type:"CHANGE_USER_NAME",
    payload:{           //将荷载的数据放置到payload中
        userName:"老张"
    }
})
console.log(store.getState())   //老张
```

**优化为解构赋值**

```
import {
    createStore
} from "redux";
const initState={
    userName:"老李"
}
const reducer = function(state=initState,{type,payload}){
    state=JSON.parse(JSON.stringify(state))
    if(type==="CHANGE_USER_NAME")
        // state.userName="老王"
        state.userName=payload.userName
    return state
};
const store = createStore(reducer);
const state = store.getState();
console.log(state)   //老李
store.dispatch({        //这个大括号里面的就是action
    type:"CHANGE_USER_NAME",
    payload:{           //将荷载的数据放置到payload中
        userName:"老张"
    }
})
console.log(store.getState())   //老张
```

###### <span style="color:red">注意：</span>

reducer当中不允许出现异步操作

每一个项目只允许有一个store

每一个store允许有多个reducer，每一个reducer可以认为是你的一个模块



###### 将一个仓库多个reducer的合并

利用combinReducers

```
import {
    createStore,
    combineReducers
} from "redux"
const reducerOne = function(state={age:1},{type,payload}){
    state = JSON.parse(JSON.stringify(state))
    return state
}
const reducerTwo = function(state={num:2},{type,payload}){
    state = JSON.parse(JSON.stringify(state))
    return state
}
const routReducer = combineReducers({
    one:reducerOne,
    teo:reducerTwo
})
const store = createStore(routReducer)
const state= store.getState()
console.log(state)
```

