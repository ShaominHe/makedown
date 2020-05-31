## VUE

读音同view    是基于js的渐进式框架

先从官网上下载vue.js文件，再引入

### 语法 

1、多关注数据，少关注视图

2、vue于js不是完全互通的

3、vue是通过数据进行驱动的



**vue特点**：MVVM    M是数据V是视图VM是vue的实例可以将数据和视图进行修改

1、双向绑定  只要m和v有一个发生变化，另一个就会通过vm发生变化

2、虚拟DOM  通过指令将虚拟DOM转为原生的Dom

#### 指令

对HTML元素属性进行的一个扩展，以v-来头

##### 1、创建vue实例

```
new Vue({
	el:"",     //指定要挂载的元素
	data:{		//用于存放数据
	
	}
})

输出：{{ }}  
el 可以指定dom元素，可以指定id名，class名，可以指定标签名。不能指定body和html
```

例：

```
<script src="../vue.js"></script>
<body>
<div id="wrap">
    <input v-model="str" type="text">
    <div>{{str}}</div>
</div>
</body>
<script type="text/javascript">
new Vue({
    el:"#wrap",
    data:{
        str:""
    }
})
得到的结果就是只要input框的内容发生改变，div的内容也会随之变化
```

如果一个VUE中绑定了多个el只有最后一个有效，一个页面可以使用多个vue，如果有多个vue绑定相同的元素，只有第一个有效

##### 2、指令

###### v-model

```
v-model   用于表单元素，可以将你的数据进行绑定
	修饰符：
		number：可以将绑定的值设定为number类型
		trim:去除左右空格
		lazy:当失去焦点时才会对数据进行响应
```

###### v-if与v-else-if与v-else

```
v-if:值是一个布尔值，如果满足条件则渲染，否则不渲染
v-else-if:值也是一个布尔值，如果满足条件渲染，否则不渲染，需要与v-if结合使用
v-else：当v-if和v-else-if都不满足时，使用
```

###### v-for

```
v-for  循环

<!-- 数组 -->
    <!-- <div v-for="item in arr">{{item}}</div>    相当于遍历数组 结果为1 2 3 4 5 -->
    <!-- <div v-for="(item,index) in arr">{{item}}:{{index}}</div>    得到值和下标，第一个是值第二个是下标 结果为1:0  2:1  3:2  4:3  5:4-->

<!-- 对象 -->
    <!-- <div v-for="item in obj">{{item}}</div>   遍历对象  结果是张三 12 -->
    <!-- <div v-for="(value,key) in obj">{{value}}:{{key}}</div>   第一个参数是变量值，第二个是变量名 -->

<!-- 数字 从1开始-->
    <!-- <div v-for="n in num">{{n}}</div>   遍历数字  结果是  从1到10 -->

<!-- 字符串 -->
    <!-- <div v-for="s in str">{{s}}</div>   遍历字符串 -->
```

###### v-html与v-text

```
v-html:会将元素的内容用指定的数据全部替换，可以使浏览器识别html元素
v-text:会将元素内的内容用指定的数据全部替换，数据会做为文档格式来处理
{{}}  只是输出文档，可以进行修饰
html和text在里面写入内容不会显示，html可以识别标签，text不可以
<div class="box">
    <input type="text" v-model="str">
    <div>{{str}}</div>
    <div v-html="str"></div>
    <div v-text="str"></div>
</div>
```

###### v-bind

```
v-bind:可以将你的元素属性与你的数据状态进行绑定，简写为冒号:

<div class="box">
    <img src="https://www.baidu.com/img/bd_logo1.png" alt="">
    <img v-bind:src="imgStr" alt="">
    <img :src="imgStr" alt="">
</div>
</body>
<script type="text/javascript">
new Vue({
    el:".box",
    data:{
        imgStr:"https://www.baidu.com/img/bd_logo1.png"
    }
})
```

###### v-on

```
v-on可以绑定事件，简写形式：@

<body>
    <div class="box">
        <h3>{{num}}</h3>
        <!-- 点击加1 -->
        <input type="button" value="点击" v-on:click="num++">
        <!-- 点击加2 -->
        <input type="button" value="点击" v-on:click="fn">
        <!-- 点击加3 -->
        <input type="button" value="点击" v-on:click="fn1(3)">  
        <!-- 不能直接弹出，需要写一个alert方法 -->
        <input type="button" value="点击" v-on:click="alert(4)">  
    </div>

</body>
<script type="text/javascript">
new Vue({
    el:".box",
    data:{
        num:1
    },
    methods:{
        fn(){
            this.num+=2   //this指VW
        },
        fn1(n){
            this.num+=n   //this指VW
        },
        alert(n){
            alert(n)   //this指VW
        }
    }
})
```



###### 显示与隐藏:v-show与v-if

```
v-show:通过display来控制你的显示与隐藏，值是一个布尔，
v-if：决定是否渲染
```

###### v-once与v-pre

```
v-once将你的数据呈现到元素当中，如果该元素再次发生变化，不会受其影响
v-pre:可以将元素内的数据忽略

<div id="wrap">
    <input type="text" v-model="str">
    <div>{{str}}</div>
    <div v-once>{{str}}</div>
    <div v-pre>{{str}}</div>   //输出{{str}}
</div>
</body>
<script type="text/javascript">
new Vue({
    el:"#wrap",
    data:{
        str:"你好"
    }
})
```

###### v-style

```
<body>
<div id="myApp">
    <p style="background:red;color:green;">你过来呀！</p>
    <p :style="'background:red;color:green;'">你过来呀！</p>
    <p :style="ps">你过来呀！</p>
    <p :style="one">你过来呀！</p>
    <p :style="{background:'greenyellow',color:'orange'}">你过来呀！</p>
    <p :style="{...bg,...co}">你过来呀！</p>
    <p :style="[bg,co]">你过来呀！</p>
</div>
</body>
<script>
    new Vue({
        el:"#myApp",
        data:{
            ps:"background:red;color:green;",
            one:{
                background:"gold",
                color:"blue"
            },
            bg:{
                background:"yellow"
            },
            co:{
                color:"blue"
            }
        }
    });
```

###### v-class

```
<style>
    .bg{
        background:red;
    }
    .co{
        color:yellow
    }
</style>
<script src="../vue.js"></script>
<body>
<div class="wrap">
    <p class="bg">起来</p>
    <p class="bg co">起来</p>
    <p :class="'bg co'">起来</p>
    <p :class="one">起来</p>
    <p :class="['bg','co']">起来</p>
    <p :class="{'bg co':true}">起来</p>
    <p :class="{'bg co':false}">起来</p>
</div>
</body>
<script type="text/javascript">
new Vue({
    el:".wrap",
    data:{
        one:"bg co"
    }
})
```

![1571317320917](C:\Users\heshaomin\AppData\Roaming\Typora\typora-user-images\1571317320917.png)

###### v-cloak

```
可以解决闪烁出来的源代码

在所需要设置的div中加入v-cloak
设置css样式

<style>
    [v-cloak]{  
    display: none;
}
</style>
<body>
    <div id="wrap" v-cloak> </div>
```

###### v-pre

```
可以直接输出{{}}   不管定不定义，都是输出大括号的内容
如：
<div>{{str}}</div>   //结果是{{str}}
```

###### key

```
<div id="wrap">
    <div v-for="item in goodList" :key="item.goodsId">   
        <!-- key是当你更新数据的时候，根据你的key和之前的key比较，如果一样的话，不会重新解析，提升效率 -->
        <h3>{{item.goodsName}}</h3>
        <p>价格：￥{{item.goodsPrice.toFixed(2)}}</p>
        <p>价格：{{currency(item.goodsPrice)}}</p>
        <p>上架时间：{{date(item.addTime)}}</p>
        <hr>
    </div>
</div>
```

##### 表单

###### 下拉

```
<div id="myApp">
    <input type="text" v-model="city">
    <input type="button" value="点击" @click="fn">
    <select v-model="city">
        <option value="1">河南</option>
        <option value="2">河北</option>
        <option value="3">山东</option>
        <option value="4">山西</option>
    </select>
    <div>{{city}}</div>
</div>
</body>
<script type="text/javascript">
new Vue({
    el:"#myApp",
    data:{
        city:"1"
    },
    methods:{
        fn(){
            this.city=4
        }
    }
})
```

###### 单选多选

```
<div id="wrap">
    <div>
        <input type="checkbox" value="1" v-model="hobby">学习
        <input type="checkbox" value="2" v-model="hobby">吃饭
        <input type="checkbox" value="3" v-model="hobby">睡觉
    </div>
    <div>
        <input type="radio" value="1" v-model="sex">男
        <input type="radio" value="2" v-model="sex">女
        <input type="radio" value="3" v-model="sex">老
        <input type="radio" value="4" v-model="sex">少
        {{sex}}
    </div>
    
</div>
</body>
<script type="text/javascript">
new Vue({
    el:"#wrap",
    data:{
        sex:1,
        hobby:true,   //直接全选
        hobby:1,	//直接全选
        hobby:["1"]			//可以选中第一个
    }
})
```

##### 阻止默认事件

###### 事件

```

1、你在不传值的情况下，括号可以省略，默认事件绑定的方法当中的第一个参数是事件对象

2、如果你传递参数的话，事件对象默认是不会传递过去的，需要手动传递，可以通过$event进行传递
	修饰符：
		.stop可以阻止冒泡
		.prevent可以阻止默认行为
```



##### 知识点

```
_id:mongodb.ObjectId(id)     数据库内存放的id是这种形式所以需要转换数据类型
try{

}catch{

}
```



##### 3、过滤器

```
过滤器：
    通过过滤器对数据进行过滤。
    1、如何注册过滤器。
        filters:{
            one(){
                return
            }
        }
    2、如何使用
        {{num | one}}
```



```

局部过滤器

<div id="wrap">
{{num | addOne(5)}}
</div>  
<div id="two">
    {{num | addOne(10)}}
</div> 
</body>
<script type="text/javascript">
new Vue({
    el:"#wrap",
    data:{
        num:1
    },
    methods:{

    },
    filters:{
        addOne(v,n){
            return v+n
        }
    }
})
new Vue({
    el:"#two",
    data:{
        num:10
    },
    methods:{

    }, 
    filters:{    //这个filters如果不写调用上面那个会报错，体现了他的局部性
        addOne(v,n){
            return v+n
        }
    }
})
```



```

全局过滤器：

<div id="wrap">
        {{num | addOne(3)}}
</div>
<div id="two">
    {{num | addOne}}
</div>   
</body>
<script type="text/javascript">
//第一个参数是过滤器的名字，第二个参数是一个函数，v是要过滤的内容，n是自己要传的值
Vue.filter("addOne",function(v,n=1){
    return v+n;
})
new Vue({
    el:"#wrap",
    data:{
        num:1
    },
    methods:{

    }
})
new Vue({
    el:"#two",
    data:{
        num:100
    },
    methods:{

    }
})
```



##### 4、es6

```
1、导出
    export const a = 12;
2、引入
    <script type="module">
        import {a} from "./module/one.js";
        console.log(a);
    </script>
```



```
当引入多个模块时，名字冲突可以使用as
1、导出
    two.js
        export let userName = "laotie";
        export let age = 13;
        export let sex = "男";
    two2.js
        export let userName = "two2.js"
2、引入
    <script type="module">
        import {userName,age,sex} from "./module/two.js";
        import {userName as twoUserName} from "./module/two2.js"
        console.log(userName,age,sex,twoUserName)
    </script>
```



```
// 当返回的属性较多时或我要使用的属性比较多。
1、导出
    two.js
        export let userName = "laotie";
        export let age = 13;
        export let sex = "男";
    two2.js
        export let userName = "two2.js"
2、引入
     <script type="module">
        import * as my from "./module/two.js";  //用as可以引入所有的导出模块
        import {userName} from "./module/two2.js"
        console.log(my.userName,my.age,my.sex,userName);
     </script>
```



```

默认导出：export default 只能默认导出一次，否则会报错

1、导出
    // 默认导出   数组，对象，函数都可以导出
    export default {
        a:1,
        b:2,
        c:3
    }
2、导入
    <script type="module">
        import three from "./module/three.js";
        console.log(three.a,three.b,three.c)
    </script>
```



```
混合导出：
1、导出
four.js
    export const userName = "laoli";
    export default {
        age:123
    }
2、导入
    <script type="module">
        import four,{userName} from "./module/four.js"
        console.log(four.age,userName)
    </script>
```



```

导出：
four.js
    console.log(1111)
    export default{
        age:13
    }
    export const userName="laoli"
five.js
    console.log(1111)
    import four from "./four.js"
    export default four
引入：
<script type="module">
    import four from "./four.js"
    import five from "./five.js"
    console.log(five===four)     //true  而且只有一个输出1111
</script>

当一个模块被引入多次时，每次得到的值都是引入第一次的值。模块只会运行一次。
```



##### 5、计算属性

对于复杂的逻辑操作应该使用计算属性，因为计算属性会将数据放到缓存中提升效率。

函数的名字，即是计算属性的名字，计算属性的名字直接在Vue当中进行输出

```
<div id="myApp">
    <input type="text" v-model="str">
    <!--<p>{{fn(str)}}</p>-->  //当有很多标签同事调用fn时，只要input框发生变化，就会输出111
    <!--<p>{{fn(str)}}</p>-->  
    <p>{{reverseStr}}</p>  //添加到缓存中，只有当结果收到影响时才会执行
    <p>{{reverseStr}}</p>
    <p>{{reverseStr}}</p>
    <p>{{reverseStr}}</p>

</div>
</body>
<script>
    new Vue({
        el:"#myApp",
        data:{
            str:"我不困，我喜欢学习"
        },
        methods:{
            fn(v){    //当有很多标签同事调用fn时，只要input框发生变化，就会输出111
                console.log(111111111);
                return v.split("").reverse().join("");
            }
        },
        computed:{
            // 函数的名字，即是计算属性的名字，计算属性的名字直接在Vue当中进行输出
            reverseStr(){    //添加到缓存中，只有当结果收到影响时才会执行
                console.log(222222222);
                return this.str.split("").reverse().join("")   //只要str不发生变化就不会执行
            }
        }
    });
</script>
```



##### 6、侦听

侦听某个数据是否发生变化，当数据发生变化时，执行相对应的操作

函数的名字即是你要侦听的数据的名字。当数据发生变化时，该函数会执行

###### 非深度侦听

```
<div id="myApp">
    <!-- <input type="text" v-model.number="num">{{num2=num+1}} -->
    <input type="text" v-model.number="num">{{num2}}

</div>
</body>
<script>
    new Vue({
        el:"#myApp",
        data:{
            num:1,
            num2:2
        },
        methods:{
        },
        watch:{
            // 函数的名字即是你要侦听的数据名字，当数据发生变化时，该函数会执行
            //有两个参数，newValue是修改之后的值,oldValue是修改之前的值
            num(newValue,oldValue){
                console.log(1211)
                //this.num2=this.num+1
                this.num2=newValue+1
            }
        }
    });
</script>
```

###### 深度侦听

```
如果data里面的数据是对象则不是深度侦听，侦听不到变化

<div id="myApp">
    <input type="button" value="修改" @click="obj={}">
    <input type="text" v-model="obj.userName">
    <div>{{obj.userName}}</div>

</div>
</body>
<script>
    new Vue({
        el:"#myApp",
        data:{
            obj:{
                userName:"laowang"
            }
        },
        methods:{
        },
        watch:{
            // obj(){
            //     console.log(111)   //除非obj的引用地址改变才会侦听到，比如写个按钮将obj变为空
            // }
            obj:{
            	//句柄
                handler(newValue,oldValue){  //这两个handler和deep是固定名字不能变
                    console.log(1111)
                    //里面写侦听到之后执行的内容
                    //此时的修改前和修改后值是相同的，因为地址没变
                    console.log(newValue.userName,oldValue.userName)  
                },
                deep:true   //是否深度侦听
            }
        }
    });
```

另一种写法（了解）

```
const vm=new Vue({
    el:"#myApp",
    data:{
    num:1,
    obj:{
    	age:12
    }
    },
    methods:{

    }
})
//第一个参数是你要侦听的数据，第二个参数是一个函数（当数据变化时执行该函数），第三个参数是一个对象，绝对是否要深度侦听
vm.$watch("num",function(newVlaue,oldVlaue){
	console.log(newVlaue,oldVlaue,age)
},{
	deep:true
})
```

##### 计算属性与侦听的区别：

计算属性：重点在结果，在计算

侦听：当某个数据发生变化时，



#### 小总结

```
new Vue({
	el:"",
	data:{
	
	},
	methods:{
	
	},
	filters:{
	//
	},
	computed:{
	
	},
	watch:{
	
	}
})
```

一般过滤器都是使用外部过滤器