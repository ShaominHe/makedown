##### 补充知识点：

函数没有返回值，得到的是undefined，

函数接受的参数没有传递，得到的是undefined，

解构赋值没有相应的项，得到的是undefined，

对象没有相对应的属性，得到的是undefined

### git

是一个开源分布式的版本管理工具

git   --version     查看版本

##### 设置：

git  config   --global   user.name   "heshaomin"

git  config   --global   user.email   "heshaomin@163.com"

##### 查看：

git   config   user.name

git   config   user.email



****************************************

#### git三大件:

1、仓库：存储项目的版本

2、暂存区（缓存区）： 代码不允许直接放置到仓库，而是需要通过暂存区

3、工作区：开发项目的地方

**************************************************

#### git命令：

git   init：   	初始化仓库

git   status:       查看文件状态

​				红色的字代表的是未添加到暂存区。

​				绿色的字代表的是添加到暂存区，但未添加到仓库

git  diff   文件名:         查看指定文件与上一版本的差异

​				红色字代表的是删除的内容

​				绿色字代表的是增加的内容

​				白色字代表的是未修改的内容

git   reset   HEAD   文件名：将指定的文件从暂存区拉回到工作区

git   reset   HEAD   *：将暂存区所有的文件拉回到工作区

**1、增加：**

​		1、git   add

​				1、git   add   index.html:   	将index.html放置到暂存区

​				2、git   add   index.html   index.js :   	将多个文件放置到暂存区，文件之间用空格分隔

​				3、git   add   *: 		将所有新增加的以及修改的文件放置到暂存区

​		2、git   commit   -m   "说明":    提交到仓库，同时会生成版本信息

**2、修改**

​		1、git   add   *

​		2、git   commit   -m    "说明"

​	场景：修改的过程中，想要将代码恢复到上一个版本

​		git   checkout  文件名

**3、删除**

​	1、物理删除：点击删除或shift+delete+回车

​			1、如果是误删除，需要找回

​					git   checkout   文件名

​			2、真删除，需要让仓库与其同步

​					git   rm   文件名

​					git   commit   -m   "说明"

​	2、命令删除:

​			1、误删除

​					git   rm   文件名

​					git   reset   HEAD   文件:    将操作从暂存区当中清除

​					git   checkout   文件名:   将上一个版本的文件拉取到工作区

​			3、真的删除

​					git   rm   文件名：

​					git   commit   -m   "说明"

***********************************

版本的切换

​			git   reflog   操作记录    操作ID

​			切换：

​						git   reset   --hard    版本号的前4位

​						git   reset   --hard   HEAD^:恢复到上一个版本

******************************************

分支：主分支  master

​		1、git   branch:	查看当前分支

​		2、git   branch  --all:   查看所有分支

​		3、git   branch   dev:   创建一个dev分支

​		4、git   checkout   dev:   进入到dev分支

​		5、git   checkout   -b   test:  创建一个名字为test的分支，并进入到该分支

合并：

将dev合并到master                 

1、进入master

​			git   checkout   master

2、合并

​			git   merge   dev:将dev合并到当前分支

3、合并冲突如何解决

​		1、手动合并

​		2、git   add   *

​		3、git   commit   -m   "说明"

**********************************

#### 远程仓库：

1、设置SSH  KEY

​		ssh-keygen   -t   rsa   -C   "heshaomin000@163.com"

​		1、在cmd上运行以上命令

​		2、打开C:\Users\heshaomin/.ssh/id_rsa.pub

​		3、将该文件的内容复制到github的设置里的秘钥中

2、创建仓库

3、将项目放置到远程仓库

第一次提交

​	git push  -u  origin  分支

以后直接

​	git  push

拉取

​	git  pull

修改之后

add     commit    push

4、解决冲突

​		1、git    pull

​		2、合并代码

​		3、git   add

​		4、git   commit   -m   "说明"

​		5、git   push

克隆dev分支

git   clone   -b   dev   git@github.com:heshaomin971019/1916.git

**************************

团队协作：

1、邀请对方

2、对方打开自己邮箱接收邀请

3、对方可以直接操作你的项目了

****************************

git 流程

1、负责人

master:

​		1、创建一个react项目

​		2、可以在该项目当中增加一些store,views,components,router,assets,setupProxy等文件

​		3、创建本地仓库：git   init

​		4、将项目提交到本地仓库

​					git   add   *

​					git   commit   -m   "说明"

​		5、与远程创建关联

​				git   remote    add    origin    仓库地址

​		6、提交

​				git   push   -u   origin   master

​	dev分支

​	1、git   checkout   -b   dev

​	2、git   push   -u   origin   dev

2、替他人

克隆dev分支

git   clone   -b   dev   git@github.com:tydtea/1916.git

**************************

clone和pull的区别就是

pull得在有本地仓库的情况下，才能pull拉取下来

clone是没有仓库的时候克隆下来