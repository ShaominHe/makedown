### 	JSONP

#### 补充

##### 键盘事件

可以在事件后面加修饰符，键盘事件有事件对象e

```
<div id="wrap">
<!-- <input type="text" @keyup="fn"> -->
<!-- <input type="text" @keyup.65="fn"> -->
<!-- <input type="text" @keyup.a="fn"> -->
<!-- <input type="text" @keyup.17="fn">       ctrl键-->  
<input type="text" @keyup.left="fn">
</div>   
</body>
<script type="text/javascript">
new Vue({
    el:"#wrap",
    data:{
        
    },
    methods:{
        fn(e){
            console.log(e.keyCode,e.target.value)
        }
    }
})
```



接口的地址，js的地址并不是以扩展名为唯一依据

将obj转换为a=1&b=2，object.key是将对象的属性名组成一个数组

```
const obj={
	a=1,
	b=2
}
console.log(Object.key(obj).map(v=>v+"="+obj[v]).join("&"))
```

### 模板

```

模板会将你指定的div覆盖掉
模板当中的标签，有且只能有一个根元素

<div id="wrap">
    啊啊啊啊
</div>   
</body>
<script type="text/javascript">
new Vue({
    el:"#wrap",
    data:{
        
    },
    //模板会将你指定的div覆盖掉
	//模板当中的标签，有且只能有一个根元素
	//建议使用反引号
    template:"<div>啦啦啦</div>"
})
```

第一种写法：直接定义

```
<div id="wrap">
    啊啊啊啊
</div>   
</body>
<script type="text/javascript">
new Vue({
    el:"#wrap",
    data:{
        str="我爱你中国"
    },
    template:`
    <div>
    	<input type="text" v-model="str">
    </div>
   `
})
```

第二种写法：通过script定义

```
<body>
    <div id="wrap">
        
    </div>
</body>
//模板定义
<script type="x/template" id="one">
    <div>
        one
    </div>
</script>
<script type="text/javascript">
    new Vue({
        el: "#wrap",
        data: {
        },
        methods:{

        },
        template: `#one`     //使用
    })
</script>
```

第三种写法：通过Vue提供的内置组件。（组件：Vue中对你的元素标签的扩展）

```
<body>
    <div id="wrap">

    </div>
    //定义
    <template id="one">
        <div>
            我爱你{{str}}
        </div>
    </template>
</body>
<script type="text/javascript">
    new Vue({
        el: "#wrap",
        data: {
            str:"我不爱你了"
        },
        methods:{

        },
        template: `#one`    //使用
    })
</script>
```

### 自定义组件：

对你标签元素的扩展

```
<body>
    <div id="wrap">
        <!-- 如果下面定义组件的时候出现大写时需要用-链接
        但是如果直接闭合下面的内容不会执行-->
        <Lao-Shi></Lao-Shi>
        <lao-shi></lao-shi>
        <Lao-shi></Lao-shi>
        <lao-Shi></lao-Shi>
        <!-- 直接闭合下面的内容不会执行 -->
        <laoshi/>
        <la-ji></la-ji>
        <la-Ji></la-Ji>
        <t-w-o></t-w-o>
        <T-W-O></T-W-O>
        <one></one>
        <one></one>
        <one></one>
        <one></one>
        <one></one>
    </div>
</body>
<script type="text/javascript">
    new Vue({
        el: "#wrap",
        data: {
        },
        methods:{

        },
        //组件
        components:{
            //属性名即使组件名，值是一个对象，
            //vue中的data,methods,template,watch,等等这里都有，但是没有el所以必须要有一个模板
            one:{
                template:`
                    <div>啊啊啊啊啊啊啊</div>
                `
            },
            TWO:{
                template:`
                    <div>呀呀呀呀呀呀呀</div>
                `
            },
            laJi:{
                template:`
                    <div>踩踩踩踩踩踩</div>
                `
            },
            LaoShi:{
                template:`
                    <div>大大大大大大</div>
                `
            }
        }
    })
</script>
```

组件里面的内容时独有的，不能直接引用外面的内容
组件里面的data必须是一个函数，函数必须要有返回值，返回值必须是一个对象，这样可以保证组件与组件之间不会相互影响

**组件的三种写法**

第一种

```
<body>
    <div id="wrap">
        <book-list></book-list>
    </div>
    <template id="tp">
        <div>
            啊啊啊啊
            <h3 v-for="item in bookArr">《{{item}}》</h3>
        </div>
    </template>
</body>
<script type="text/javascript">
    new Vue({
        el: "#wrap",
        data: {
        },
        methods:{

        },
        components:{
            bookList:{
                data(){
                    return {
                        bookArr:["一万个为什么","坏蛋是怎样练成的"]
                    }
                },
                template:`#tp`
            }
        }
    })
```

第二种：

```
<body>
    <div id="wrap">
        <book-list></book-list>
    </div>
    <template id="tp">
        <div>
            啊啊啊啊
            <h3 v-for="item in bookArr">《{{item}}》</h3>
        </div>
    </template>
</body>
<script type="text/javascript">
    const bookList = {
        data() {
            return {
                bookArr: ["一万个为什么", "坏蛋是怎样练成的"]
            }
        },
        template: `#tp`
    }
    new Vue({
        el: "#wrap",
        data: {
        },
        methods: {

        },
        components: {
            bookList
        }
    })
```

第三种

```
<body>
    <div id="wrap">
        <book-list></book-list>
    </div>
    <template id="tp">
       
    </template>
</body>
<script type="module">    //将模板组件都放到了外面没
    import bookList from "./组件用法外部.js"
    new Vue({
        el: "#wrap",
        data: {
        },
        methods: {

        },
        components: {
            bookList
        }
    })
    
    
    
    
  js:
  
  const template = `
    <div>
        啊啊啊啊
        <h3 v-for="item in bookArr">《{{item}}》</h3>
    </div>
`
export default {
    data() {
        return {
            bookArr: ["一万个为什么", "坏蛋是怎样练成的"]
        }
    },
    template
}

```

组件的嵌套

```
<body>
    <div id="wrap">
        <one></one>
    </div>
</body>
<script type="text/javascript">
    new Vue({
        el: "#wrap",
        data: {
        },
        methods:{

        },
        components:{
            one:{
                components:{
                    two:{
                        template:`<div>呀呀呀</div>`
                    }
                },
                template:`<div>啊啊啊<two></two></div>`
            }
        }
    })
</script>
```

