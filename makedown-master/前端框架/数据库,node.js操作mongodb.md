### 数据库

数据库：用于存放数据dba

mysql：关系型数据库（结构型数据库）

userList:

​			userId：唯一标识
​			userName:
​			sex

textList:

​			tesId:
​			userId:
​			score:
​			detail:

库

​		表

​				行

​				列

mongodb：菲关系型数据库（非结构性数据库）

库

​		集合

​				文档

#### 安装

```
mongo:进入mongo环境。
    不是内部命令。----设置：-->环境变量。
    电脑->右击属性->高级系统设置 ->高级->环境变量->系统变量：path
    windows10:
        新建：C:\Program Files\MongoDB\Server\3.4\bin
        确定。
    windows7:
        在字符串尾部追加：
            ;C:\Program Files\MongoDB\Server\3.4\bin
        确定
    **************************
    关闭控制台，再打开。
    *****************************************
    如果出现：
    		runtime.dll
    打开下列网址解决：
    https://zhangpeiyue.blog.csdn.net/article/details/88768532
```



#### 命令

```
1、挂载数据库
    1、在你的某个盘符下创建一个文件夹。比如-》d->mongo
       这个文件夹用地存放你的数据库文件。
    2、mongod --dbpath d:\mongo
    3、将控制台最小化。
    http://127.0.0.1:27017
    It looks like you are trying to access MongoDB over HTTP on the native driver port.
    4、重新打开一个新的控制台
2、通过mongo命令进入mongo环境。

***************************其它**********************************

show dbs:显示当前的所有数据库列表
use 1916:切换到数据库1916
db:查看当前数据库
show collections:查看当前数据库当中所有的集合。
**************************增*************************************
db.userList.insert({userName:"laoliu"}):在当前数据库当中的userList集合中增加一条文档。
mongoimport --db 1916 --collection score --file   --drop
    db:指定数据库
    collection:指定集合
    file:指定文件地址
    drop:是否覆盖。可选
    
**************************删*************************************

db.dropDatabase():删除当前数据库
db.score.remove({sex:"男"}):删除性别为男的
db.score.remove({sex:"女"},{justOne:true})：仅删除一条
db.score.remove({})：清空指定的集合。
db.score.drop()：删除集合。

**************************改*************************************

db.score.update({userName:"张程"},{$set:{sex:"未知"}}):将张程同学的性别修改为未知。
db.score.update({userName:"张三"},{age:10000})：完整替换为年龄10000。
db.score.update({userName:"王五"},{$inc:{age:-11}})：年龄减11
db.score.update({sex:"男"},{$set:{age:18}},{multi:true}))：将所有男同胞的年龄修改为18

**************************查*************************************

db.score.find():将当前库中的score集合中的文档进行显示。
db.score.find({userName:"赵六"})):查找用户名为赵六的信息。
db.score.find({sex:"男",age:12})：多条件查找
db.score.find({userName:/张/}) ：根据条件模糊查找。
db.score.find({age:{$ne:13}})：年龄不等于13
    $gt:大于
    $lt:小于
    $gte:大于等于
    $lte:小于等于
    $ne:不等于
db.score.find({$or:[{sex:"男"},{age:12}]})：年龄为12或性别为男
db.score.find().count():求文档的条数
db.score.find({sex:"男"}).count()：根据条件
db.score.find().sort({age:-1})：按照年龄的倒序。正序1，倒序-1
db.score.find().sort({age:-1,"score.shuxue":1})：年龄的倒序，数学正序（当年龄相同时，按照数学的正序）
db.score.find().limit(2)：获取指定集合的文档条数。
db.score.find().limit(4).sort({age:-1}).skip(8)：年龄倒序，跳过8条文档，取前4条
先排序，在跳过，在取
```

```
分页的数据
当前页：pageIndex  2
总页数：pageSum ?      Math.ceil(count/pageNum)
总条数：count ?
每页要显示的条数：pageNum 4
点击第几页显示的内容的计算公式skip = (pageIndex - 1) * pageNum;
```



#### node.js操作mongodb

```
1、下载
    cnpm install mongodb -S
2、引入
    const mongodb = require("mongodb")
    // MongoClient属性可以实现对数据库的连接
    const mongoClient = mongodb.MongoClient;   //mongodb下面的属性mongoClient属性
    //mongoClient下面的connect方法
    mongoClient.connect("mongodb://127.0.0.1:27017",functions(err,client){
        // 终端
        if(err){
            console.log("连接数据库失败");
        }else{
            const db = client.db("用的数据库的名字");   //client下面的db方法
            console.log("连接数据库成功");
        }
    })
```

```
const mongodb=require("mongodb")
const mongoClient = mongodb.MongoClient
mongoClient.connect("mongodb://127.0.0.1:27017",{useNewUrlParser: true,useUnifiedTopology: true},function(err,client){
 	if(err)
        console.log("连接失败")
    else
        console.log("连接成功")
        const db=client.db("he");
        //增加一条数据
        // db.collection("userList").insertOne({userName:"啊啊"},function(err,results){
        //     if(err)
        //         console.log("连接失败");
        //     else
        //         console.log("连接成功");
        // })

        //增加多条数据
        // db.collection("userList").insertMany([{userName:"啊啊啊"},{userName:"aaaaaa"}],function(err,results){
        //     if(err)
        //         console.log("连接失败")
        //     else
        //         console.log("连接成功")
        // })

        //查询多条数据   find查找要用toArray(function(err,results){})
        // db.collection("userList").find({userName:"啊啊"}).toArray(function(err,results){
        //     console.log(results)
        // })

		//查询一条数据
        // db.collection("userList").find({userName:/啊/}).toArray(function(err,results){
        //     console.log(results)
        // })

		//查询一条数据
        // db.collection("userList").findOne({userName:/啊/},function(err,results){
        //     console.log(results)
        // })

		//查询一条数据
        // db.collection("userList").findOne({userName:new RegExp("啊")},function(err,results){
        //     console.log(results)
        // })
})
```

