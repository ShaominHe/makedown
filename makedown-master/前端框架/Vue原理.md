### Vue原理

```
第一个参数是你要拦截的对象，第二个参数是你要拦截对象的属性，第三个参数是对拦截进行描述（设置）
Object.defineProperty()
```

例：

```
const obj={}
Object.defineProperty(obj,"userName",{
	get(){
	//读取
	},
	set(){
	//写入
	}
})
obj.userName="老铁";
console.log(obj.userName)
```

