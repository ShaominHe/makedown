### promise

```
function fn() {
    // async await
    // Promise.all();// 同时执行多个Promise
    //Promise.race();// 得到的是最快执行完的promise结果
    return new Promise((resolve, reject) => {
        if (1 === 1) {
            resolve("success");
        }else{
            reject("error");
        }
    })
}
第一种：
fn().then(data=>{    //成功的时候执行
    console.log(data);
}).catch(err=>{
    console.log(err);
})

第二种：
fn().then(data=>{   //失败的时候执行
	console.log(data);
},err=>{
	console.log(err);
})
```



```
链式写法

new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve("学习完毕")
    },1000)
}).then(v=>{
    console.log(v)
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve("投完简历")
        },1000)
    })
}).then(v=>{
    console.log(v)
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve("面试结束")
        },1000)
    })
})
```

进一步优化    //写成函数可以传递参数，优化代码

```
 function one() {    
     return new Promise((resolve, reject) => {
         setTimeout(() => {
             resolve("学习完毕")
         }, 1000)
     })
 }
 function two() {
     return new Promise((resolve, reject) => {
         setTimeout(() => {
             resolve("投完简历")
         }, 1000)
     })
 }
 function three() {
     return new Promise((resolve, reject) => {
         setTimeout(() => {
             resolve("面试结束")
         }, 1000)
     })
 } 
 function four() {
     return new Promise((resolve, reject) => {
         setTimeout(() => {
             resolve("上班了");
         }, 1000)
     })
 }
 
 
 one().then(v => {
        console.log(v)
        return two();
    }).then(v => {
        console.log(v)
        return three();
    }).then(v => {
        console.log(v);
        return four();
    }).then(v => {
        console.log(v);
    })
```

进一步优化

```
function fn(str) {
    return new Promise((resolve,reject)=>{
        setTimeout(() => {
            resolve(str);
        },1000)
    })
}
fn("学习结束").then(v => {
        console.log(v)
        return fn("投完简历了");
    }).then(v => {
        console.log(v)
        return fn("面试结束");
    }).then(v => {
        console.log(v);
        return fn("上班了");
    }).then(v => {
        console.log(v);
    })
```

再优化

```
function fn(str) {
    return new Promise((resolve,reject)=>{
        setTimeout(() => {
            resolve(str);
        },1000)
    })
}
async function my() {
    const oneStr =await fn("学习结束");
    console.log(oneStr);
    const twoStr = await fn("投简历结束");
    console.log(twoStr);
    const threeStr = await fn("面试结束");
    console.log(threeStr);
    const fourStr = await fn("上班");
    console.log(fourStr);
}
my();
```





补充：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
<script>
    const num = 0;
    function loadImg(imgSrc) {
        return new Promise((resolve,reject)=>{
            const img = new Image();
            img.src = imgSrc;
            img.onload = function () {   //图片加载成功执行的函数  异步操作
                setTimeout(()=>{   //设置一个随机数的延时，利用promise.race谁快加载谁
                    resolve(img);
                },Math.random()*2000)

            }
            img.onerror = function () {	  //图片加载失败执行的函数  异步操作
                reject("加载失败")
            }
        })
    }
    // 比赛。执行多个promise.哪一个promise先执行完，就得到哪一个的结果
    Promise.race([loadImg("./1.png"),loadImg("./2.jpg")]).then(img=>{
        document.body.appendChild(img)
    })
    // 可以请求多种异步操作,会将多个导步的结果生成一个数组。当其中任意一个promise失败的话，不会向下执行，返回失败。
    // Promise.all([loadImg("./1000.png"),loadImg("./2.jpg")])
    //     .then(imgArr=>{
    //         imgArr.forEach(img=>document.body.appendChild(img))  //遍历数组
    //         console.log(imgArr)
    //     }).catch(err=>{
    //         console.log(err)
    //     })


    // loadImg("./1.png").then(img=>{
    //     document.body.appendChild(img);
    // }).catch(err=>{
    //     console.log(err);
    // })
    // loadImg("./2.jpg").then(img=>{  
    //     document.body.appendChild(img);
    // }).catch(err=>{
    //     console.log(err);
    // })

    // const img = new Image();
    // img.src = "./1.png";
    // img.onload = function () {   //图片加载成功执行的函数  异步操作
    //     console.log("加载成功")
    //     document.body.appendChild(img);
    // }
    // img.onerror = function () {   //图片加载失败执行的函数  异步操作
    //     console.log("加载失败")
    //     img.src = "./2.jpg"
    // }
    // console.log(2222222);



</script>
</html>

```

### 补充

```
try{

}catch(err){

}finally{

}

当try发生错误时，执行catch，finally不管有没有错误都会执行
当try没有错误可以执行时，catch不会执行

判断是否为整形：
Number.isInteger(str)   //判断str是否为整形，返回布尔值
```

