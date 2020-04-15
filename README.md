https://www.cnblogs.com/nongzihong/p/12421178.html
windows下MongoDB的安装及配置
1.下载上mongodb官网

https://www.mongodb.com/download-center/community

下载自己电脑对应的版本

 

2.安装



 

 

 自定义 安装路径修改下   D:\MongoDB 

然后不断“下一步”，安装至结束。



 

 

 



 

 

 

下一步安装 "install mongoDB compass" 不勾选（当然你也可以选择安装它，可能需要更久的安装时间），

MongoDB Compass 是一个图形界面管理工具，我们可以在后面自己到官网下载安装

官网下载 https://robomongo.org/download 

我使用 Robo 3T 1.3.1



 

 

 

 

 

3、先创建数据库文件的存放位置

首先要在MongoDB的data文件夹里新建一个db文件夹和一个log文件夹：



然后在log文件夹下新建一个mongo.log：

 

 

 

然后将D:\MongoDB\bin添加到环境变量path中，此时打开cmd窗口运行一下mongo命令，出现如下情况：



 

 

 这是为什么呢？这是因为我们还没有启动MongoDB服务，自然也就连接不上服务了。那要怎么启动呢？在cmd窗口中运行如下命令：

 mongod --dbpath D:\MongoDB\data\db
需要注意的是：如果你没有提前创建db文件夹，是无法启动成功的。运行成功之后，我们打开浏览器，输入127.0.0.1:27017，看到如下图，就说明MongoDB服务已经成功启动了。



 

 

 

 

如果嫌麻烦，可以选择用命令net start mongodb来手动启动，具体方法如下。

还是打开cmd窗口，不过这次是以管理员身份运行，然后输入如下命令：

mongod --dbpath "D:\MongoDB\data\db" --logpath "D:\MongoDB\data\log\mongo.log" -install -serviceName "MongoDB"
如果没有报错的话就说明成功添加到服务里了，可以使用win+R然后输入services.msc命令进行查看：



 

 

默认是自动运行的，这里我选择把它改成手动的。然后在cmd窗口中运行net start mongodb：

 

 

 

 

怎么解决呢？两个步骤：

1）运行sc delete mongodb删除服务；

2）再运行一次配置服务的命令：

mongod --dbpath "D:\MongoDB\data\db" --logpath "D:\MongoDB\data\log\mongo.log" -install -serviceName "MongoDB"
然后再运行net start mongodb，服务启动成功：



 

 

 

有可能遇到问题

1.mongod不是内部或外部命令

出现这种问题说明你没有把bin目录添加到环境变量之中，重新添加一下即可解决。

2.服务名无效

首先是看你输入的服务名称是否有误，然后再查看本地服务中有没有MongoDB服务，如果没有服务，则运行命令添加服务即可。

3.发生服务特定错误：100

删除db文件夹下的mongod.lock和storage.bson两个文件，若删除完之后仍然出现这种问题，用sc delete mongodb删除服务，再配置一下服务就能解决了。

 

其他解决方式：

在Mongodb新建配置文件mongo.config

 

 



 

 

 

用记事本打开mongo.config  ，并输入：

dbpath=D:\MongoDB\data\db
logpath=D:\MongoDB\data\log\mongo.log

 

用管理员身份打开cmd:

可能还有很多人不会管理员身份打开cmd。这也介绍下：

在下图路径下找到cmd 的运行文件

C:\Windows\System32



 

 然后右键，以管理员身份运行

 

配置windows服务：

cmd先跳转到D:\MongoDB\bin目录下。

输入：mongod --config "D:\MongoDB\mongo.config" --install --serviceName "MongoDB"

即根据刚创建的mongo.config配置文件安装服务，名称为MongoDB。



 

 启动命令：

切换到D:\MongoDB\bin目录

执行以下命令：

       mongod --dbpath D:\MongoDB\data
      这时命令行窗口会打印一些启动信息，最后一行显示为如下信息时表示启动成功了.



 

 

 

完成后，再次查看本地的服务。



 

然后浏览器 访问：



 

 

以robo3t为例简单介绍数据库的操作：
  ①robo3t，一路下一步结束。然后它默认27017端口，配置好连接就可以。



 

 



 

 点击Save 保存

创建数据库

选Create Database创建，根据需要命名数据库；



 

 

点击Create Database



 

 创建 test 为例

 



 

 

 

 

 

 



 

 

复制代码
// 往positions这个collection里插入一条数据
//增
db.getCollection('positions').insert({
    name: '前端工程师',
    age: '35',
    salary: 10000000
})
// 查
db.getCollection('positions').find()

//改：db.集合名.update({条件},{$set:{更改}})
db.positions.update(
    {name:"前端工程师"},
    {$set:{gender:"male"}
})

//删：db.集合名.remove({条件})    删除时最好有独有的属性，比如：id
db.user.positions({name: "前端工程师"})
复制代码
 

添加数据



 

 

查询数据



 

 

改：db.集合名.update({条件},{$set:{更改}})



 

 

删：db.集合名.remove({条件}) 删除时最好有独有的属性，

 

比如 db.positions.remove({name: "前端工程师"})



 

 

我们去查询一下，数据应该就被删除了，发现数据已经没了

