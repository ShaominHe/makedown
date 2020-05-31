### 项目补充知识

#### axios拦截器

请求拦截
统一配置你的请求信息

```

//使用请求拦截  config请求配置项，该回调返回的值是你最终请求的配置

axios.interceptors.request.use(function(config){
	config.url="/ele"+config.url
	return config
})
```

响应拦截
可以对数据进行二次过滤

```

//响应拦截，将返回的结果进行拦截，回调返回的值就是你最终返回的内容，返回的必须是一个对象

axios.interceptors.response.use(function({data}){
  return data
})

```

#### md5加密

1、下载

```
cnpm  install  md5  -S
```

2、引入

```
const md5 = require("md5")
```

3、使用：

```
const passWord ="123456"     //密码
const str = "$%^&*"         //颜料
console.log(md5(passWord+str))      //最终生成的md5值
```

#### token

jwt    json    web   token

流程：

1、在登陆的时候服务器负责生成token 
2、前端负责将token进行保存
3、调用私密接口时，将token放置到头部信息当中
4、服务器端负责对token进行验证

如何生成token：

1、下载

```
cnpm install jwt-simple  -S
```

2、引入

```
const jwt = require("jwt-simple")
```

3、使用

​		1、加密：生成token   荷载

```
const payload={
	adminName:"admin",
	lastTime:Date.now()+1000*60*60*2   //设置过期时间两小时
}
const key = "%^&*"    //设置一个钥匙，用来加密解密
const token = jwt.encode(payload,key)
```

​		2、解密：解析token

```
const adminInfo = jwt.decode(token,key)
```

#### 判断路由

```
 if(this.$router.name==="/shopTypeList"){
 	this.$store.dispatch("getShopTypeList")
 }else{
 	this.$router.push("/shopTypeList")
 }
```

#### 使用params

发送

```
$router.push({name:"shopList",params:{shopTypeId:scope.row._id}})
```

接收

```
this.$route.params.shopTypeId
```

#### $set

```
<template>
    <div>
        {{obj.a}}|||||{{arr}}
        <el-button @click="fn">修改</el-button>
    </div>
</template>

<script>
export default {
    name:"goods-list",
    data(){
        return {
            obj:{
                a:1
            },
            arr:[1,2,3,4]
        }
    },
    methods:{
        fn(){
            // this.obj.a=4;
            // this.arr[2]=10   //如果注释了上一句，则该语句不会执行，视图不会发生变化
            this.$set(this.arr,2,9)   //但是可以通过$set来更改
        }
    }
}
</script>
```

#### 选中的样式

##### 1、exact和active-class

在标签中加上active-class和exact可以实现选中的样式和精确点击的样式

```
<router-link class="gray" active-class="red" exact to="/">Home</router-link> |
<router-link class="gray" active-class="red" to="/discover">发现</router-link> |
<router-link class="gray" active-class="red" to="/order">订单</router-link> |
```

##### 2、在路由中设置

在router的index.js中增加linkActiveClass属性

```
const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  linkActiveClass:"red",    //通过设置路由控制样式
  routes
})
```



#### 二级路由

```
{
    path: '/discover',
    name: 'discover',
    component: Discover,
    children:[
      {
        path:"/discover",
        name:"staplefood",
        component:()=>import("@/views/discover/StapleFood")
      },
      {
        path:"/discover/complementaryfood",
        name:"complementaryfood",
        component:()=>import("@/views/discover/ComplementaryFood")
      },
      {
        path:"/discover/snacks",
        name:"snacks",
        component:()=>import("@/views/discover/Snacks")
      }
    ]
  }
```

#### 打包

1、打包上传服务器

​		1、cnpm  run  build

​		2、配置服务器

​					nginx服务器

​								1、解压缩

​								2、进入到该文件夹

​								3、start  nginx  :   开启nginx服务

​									nginx   -s  reload：刷新服务

​									nginx   -s  stop：强制停止服务

​									nginx   -s  quit：优雅的形式，会等待你的相关的请求结束以后再结束服务

​								4、代码以#号开头的是注释



     server {
             listen       80;// 端口号
             server_name  localhost;// 服务器IP
    
            location / {
                root   html;// 指定你的站点所在的文件夹
                index  index.html index.htm index.php;// 指定打开的默认文件。
            }
    
            location /ele {
            //相当于代理
                rewrite   ^/ele/(.*)$  /$1 break;
                proxy_pass   http://127.0.0.1:8088;
            }
    }
#### 上线流程

1、将nignx上传到服务器
2、nginx进行解压缩
3、将项目进行打包（后台管理，前端渲染）
4、指定站点根目录 （后台管理，前端渲染）
5、修改配置