### vuex

负责提供订单相关的路由。vuex   json   xml   xp

vuex:是一个将数据（服务杜纳，前端的数据，方法）进行集中化的工具

解决了vue组件之间的传值问题

vuex是一个仓库

1、state：数据状态。仓库当中的物品，存放数据

```
this.$store.state   来获取数据状态
```

2、getters：类似于computed，计算数据状态 ，  负责计算商品如，求和

```
this.$store.getters    来获取计算结果
使用映射简写
import {mapState,mapGetters} from 'vuex'
mapGetters(["voteSum"]),
mapState(["liuDeHua"]),
```

3、mutations：负责对数据进行同步操作		物品进行管理，修改

```
1、如果要调用mutations下的方法，需要通过 this.$store.commit('方法名',参数)
2、调用时只能传递一个参数，如果要传递多个，可以将你要传递的所有参数设置为一个对象
```

4、actions：负责异步操作			不能直接操作物品，相当于管理员

```
actions里面的函数自带一个参数，第一个参数就是自带的参数里面包含commit,dispatch,state,getters.第二个参数才是要传的参数

通过dispatch触发actions中的函数，第一个参数是函数名，第二个参数传的参数
this.$store.dispatch('getWeibo',pageIndex)  
```

4、modules：将数据进行模块化处理		将你的商品进行分类



1、使用流程

​	1、下载(安装项目时，如果选用了vuex，此步可省略)

​		cnpm install vuex -S

​	2、引入vuex

​		import  vuex  from  "vuex"

​	3、安装

​		Vue.use(vuex)

​	4、生成store

```
const  store =  new vuex.Store({
		state,
		getters,
		mutations,
		actions,
		modules
})
```

5、导出

​	export  default  store

6、main.js使用时

```
import store  from  "store"
new Vue({
	router,
	store,
	render:function(h){return h(App)}
}).$mount('#app')
```

