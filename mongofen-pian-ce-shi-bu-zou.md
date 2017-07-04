#Mongo分片研究

## 1、	场景
A：计算机的磁盘不够用了
B：数据太大，查询效率很慢，需要分片来保存，提高查询效率
C：单个mongod已经无法满足写数据的性能需要了
D：一台机器的内存大小永远有极限，想分摊内存
## 2、	分片机制
分片定义：
将数据拆分，将其分散存放在不同的机器上的过程，有时也用分区来表示。
进程定义：
mongod：mongodb服务器（mongod.exe）
mongos：路由服务器，它会根据管理员设置的“片键”将数据分摊到自己管理的mongod集群，数据和片的对应关系以及相应的配置信息保存在"config服务器"上。它路由所有的请求,然后将结果聚合.它本身并不存储数据或者配置信息（mongos.exe）
config：存储了集群的配置信息，数据和片的对应关系（mongod.exe）

## 3、	分片实践
书上建议分片数量要大于2，否则的话分片将没有什么意义，不会有性能提升。
目标：建三个分片，将192.168.1.208上的数据分片到192.168.1.8和192.168.1.182
1）	开启config服务
```
192.168.1.208：mongod --dbpath=e:\mydbtestconfig --logpath=e:\mydbtestconfig\log.txt --logappend --port 30001
192.168.1.8：mongod --dbpath=e:\mydbtestconfig --logpath=e:\mydbtestconfig\log.txt --logappend --port 30001
192.168.1.182：mongod --dbpath=c:\mydbtestconfig --logpath=c:\mydbtestconfig\log.txt --logappend --port 30001
```
2）	开启路由
这里config的配置服务器必须是1或3，2是被禁止的，命令行会提示出错。

192.168.1.208： mongos --port 30002 --
```
configdb=192.168.1.208:30001,192.168.1.8:30001,192.168.1.182:30001 --logpath=e:\mydbtestroute\log.txt --logappend
```

192.168.1.8： mongos --port 30002 --
```

configdb=192.168.1.8:30001,192.168.1.208:30001,192.168.1.182:30001 --logpath=e:\mydbtestroute\log.txt –logappend
```

192.168.1.182： mongos --port 30002 --
```
configdb=192.168.1.8:30001,192.168.1.208:30001,192.168.1.182:30001 --logpath=c:\mydbtestroute\log.txt --logappend
```

3）	开启数据服务进程
192.168.1.208： 
```
mongod --dbpath=e:\mydbtest --logpath=e:\mydbtest\log\log.txt --port 30000
```

192.168.1.8： 
```
mongod --dbpath=e:\mydbtest --logpath=e:\mydbtest\log.txt --logappend --port 30000
```

192.168.1.182： 
```
mongod --dbpath=c:\mydbtest --logpath=c:\mydbtest\log.txt --logappend --port 30000
```


4）	配置服务
192.168.1.8：
```
mongo 192.168.1.208:30002/admin
mongos> db.runCommand({'addshard':'192.168.1.208:30000', 'allowLocal':true })
```
返回{ "shardAdded" : "shard0000", "ok" : 1 }代表成功
```
mongos> db.runCommand({'addshard':'192.168.1.8:30000', 'allowLocal':true })
```
返回{ "shardAdded" : "shard0001", "ok" : 1 }代表成功
```
mongos> db.runCommand({'addshard':'192.168.1.182:30000', 'allowLocal':true })
```
返回{ "shardAdded" : "shard0002", "ok" : 1 }代表成功

5）	开启分片
```
mongos> db.runCommand({'enablesharding':'mydata'})
```
返回{ "ok" : 1 }代表成功
6）	指定分片的片键
这里先采用我们历史数据原有的索引来分片，原有的索引是time:1,pointname:1的组合方式，也就是下面语句中的key字段。
如果没有建立片键，先要建立，然后再执行以下语句：
```
mongos>db.runCommand({'shardcollection':'mydata.m5_data_mydata_hkhuarun','key':{'time':1, 'pointname':1}})
```

返回{ "collectionsharded" : "mydata.m5_data_mydata_hkhuarun", "ok" : 1 }代表成功

7）	查看分片状态
```
mongos> use mydata
switched to db mydata
mongos> db.printShardingStatus()
```

--- Sharding Status ---
sharding version: {
"_id" : 1,
"version" : 4,
"minCompatibleVersion" : 4,
"currentVersion" : 5,
"clusterId" : ObjectId("55cd98f6e934275c1a951825")
}
shards:
{ "_id" : "shard0000", "host" : "192.168.1.208:30000" }
{ "_id" : "shard0001", "host" : "192.168.1.8:30000" }
{ "_id" : "shard0002", "host" : "192.168.1.182:30000" }
databases:
{ "_id" : "admin", "partitioned" : false, "primary" : "config" }
{ "_id" : "mydata", "partitioned" : true, "primary" : "shard0000" }

mydata.m5_data_mydata_hkhuarun
shard key: { "time" : 1, "pointname" : 1 }
chunks:
shard0000 23
too many chunks to print, use verbose if you want to for
ce print


```
mongos> sh.isBalancerRunning()
```
true
过十几秒再敲命令，则如下显示：
```
mongos> db.printShardingStatus()
```
--- Sharding Status ---
sharding version: {
"_id" : 1,
"version" : 4,
"minCompatibleVersion" : 4,
"currentVersion" : 5,
"clusterId" : ObjectId("55cd98f6e934275c1a951825")
}
shards:
{ "_id" : "shard0000", "host" : "192.168.1.208:30000" }
{ "_id" : "shard0001", "host" : "192.168.1.8:30000" }
{ "_id" : "shard0002", "host" : "192.168.1.182:30000" }
databases:
{ "_id" : "admin", "partitioned" : false, "primary" : "config" }
{ "_id" : "mydata", "partitioned" : true, "primary" : "shard0000" }

mydata.m5_data_mydata_hkhuarun
shard key: { "time" : 1, "pointname" : 1 }
chunks:
shard0001 4
shard0002 3
shard0000 19
too many chunks to print, use verbose if you want to for
ce print
最终显示：
--- Sharding Status ---
sharding version: {
"_id" : 1,
"version" : 4,
"minCompatibleVersion" : 4,
"currentVersion" : 5,
"clusterId" : ObjectId("55cd98f6e934275c1a951825")
}
shards:
{ "_id" : "shard0000", "host" : "192.168.1.208:30000" }
{ "_id" : "shard0001", "host" : "192.168.1.8:30000" }
{ "_id" : "shard0002", "host" : "192.168.1.182:30000" }
databases:
{ "_id" : "admin", "partitioned" : false, "primary" : "config" }
{ "_id" : "mydata", "partitioned" : true, "primary" : "shard0000" }

mydata.m5_data_mydata_hkhuarun
shard key: { "time" : 1, "pointname" : 1 }
chunks:
shard0001 11
shard0002 10
shard0000 13
too many chunks to print, use verbose if you want to for
ce print
到此，这3个分片上的块已经趋于平衡，并且均衡器已经关闭，连接192.168.1.8、192.168.1.182、192.168.1.208任意一台电脑的mongos，均能正常访问到数据，如下图所示（连接的是192.168.1.208）。

8）	移动块
这里看到目前的分片情况是：
shard0001 11
shard0002 10
shard0000 13
也就是说shard0000上有13个块，sahrd0001上有11个块，shard0002上有10个块，可以调用moveChunk命令来移动分片：
```
mongos> sh.moveChunk('mydata.m5_data_mydata_hkhuarun',{'time':ISODate('2015-
07-25T16:35:00Z'),'pointname':'11F_VAV36_Temp'},'shard0001')
```
返回{ "millis" : 9519, "ok" : 1 }
此时再查看分片状态：
发现分片的确移动了，将shard0000上的一个块移动到了shard0001上。

9）	授权和加密
以上的步骤都是没有授权的，即不用密码就能访问所有的数据。这在实际的生产环境中是不允许的，因此要增加验证的过程。
在单机模式下，认证是通过mongod的启动参数--auth来实现的，但是在分片集群模式下，采用的是加密文件的方式（加密文件是我用openssl生成的，这里直接使用即可）。
要先在未启用授权文件的模式下添加集群用户并授权：
192.168.1.8：
```
mongo 192.168.1.208:30002/admin
mongos> db.addUser('myweb','RNB.my-2013')
mongos> db.auth('myweb','RNB.my-2013')
mongos>use mydata
mongos>db.addUser('myweb','RNB.my-2013')
```
接着关闭全部的mongod和mongos进程，并在命令中添加授权文件。
1）	开启config服务
192.168.1.208：
```
mongod --dbpath=e:\mydbtestconfig --logpath=e:\mydbtestconfig\log.txt --logappend --port 30001 --keyFile=E:\mydbtestkey\keyfile
```
2）开启mongos路由
192.168.1.208： mongos --port 30002 --configdb=192.168.1.208:30001 --logpath=e:\mydbtestroute\log.txt –logappend --keyFile=E:\mydbtestkey\keyfile
3）开启数据服务进程
192.168.1.208： 

```
mongod --dbpath=e:\mydbtest --logpath=e:\mydbtest\log\log.txt --port 30000 --keyFile=E:\mydbtestkey\keyfile
```
192.168.1.8： 
```
mongod --dbpath=e:\mydbtest --logpath=e:\mydbtest\log.txt --logappend --port 30000 --keyFile=E:\mydbtestkey\keyfile
```
192.168.1.182： 
```
mongod --dbpath=c:\mydbtest --logpath=c:\mydbtest\log.txt --logappend --port 30000 --keyFile=C:\mydbtestkey\keyfile
```
启动以上的命令后，发现再用客户端登录，如果不输入密码的话，那么就会提示没有授权，说明密码验证机制已经启动了，接着可以按照正常的使用来访问历史数据了。
10）	片键的选择
片键的选择是分片的关键，会严重影响到分片的结果和后续数据的存储，这一块内容也非常的复杂，现在讨论一下现有历史数据的片键：{‘time’:1,’pointname’:1}。
它的含义是片键有两个热点，主要的是time，其次是pointname，可以查看具体的分片信息：
{
"time" : ISODate("2015-07-16T03:35:00Z"),
"pointname" : "12F_AHU1.AHU_STA"
} -->> { "time" : ISODate("2015-07-20T01:05:00Z"), "pointname" : "11F_VAV49_Erro
r" } on : shard0000 Timestamp(1, 17)

从中可以看出，对于分片来说，先按时间来分，然后再按点名来分，如果这样的话，那么就出了大问题，新插入的历史数据永远只会位于时间最新的块上，导致这个块不断的变大，而且拆分工作也只在这个块上进行，所以数据不会均匀分配，这样就失去了分片的意义了。
所以要选择均匀的分片，我个人认为有两种片键可以供我们选择：
1）{‘pointname’:1,’time’:1}
这个片键会按点名为主来分块，然后再按照时间来划分，由于点名不是线性增长的，所以块应该来说会保持均匀的部署。
2）{‘_id’:’hashed’}
这个也叫散列片键，因为_id是随着时间而线性增加的，如果直接用_id来作为片键，那么和{‘time’:1,’pointname’:1}的效果是一样的，起不到分片的效果。如果用散列来分块，那么就会均匀分布全部的历史数据了。
对于以上两种方式，都要对全部的表创建新的索引，这是个比较耗时的工作，我试了一下，对大约有12G数据的熊猫做一次hash索引的创建，需要3个小时的时间。但是对于我们的项目来说，只有第一种方式才适合我们的历史数据，因为我们的历史数据查询使用的是点名和时间的组合索引，与_id没有什么关系。其他的集合可以采用第二种方式，让数据均匀分布在分片上。
11）	关于数据迁移
数据的迁移是一项十分消耗系统资源的工作，所以如果发生在我们的工作时间，会对用户产生影响，历史数据的查询将变得比较慢，我们可以通过设置时间再避免该问题：
```
>db.setting.update({'_id':'balancer’},{'$set':{'activeWindow':{'start':'00:00','stop':'08:00'}}})
```
这样设置了以后，均衡工作只在0点到8点之间才发生（最好是在这段时间内进行严密的监控）。
不要混合使用手动均衡和自动均衡，即使要这么做，必须格外的小心，一旦均衡器启动，你之前移动过的块，可能会再次被均衡器均衡掉，也就是你做了无用功，均衡器总是会自动的维护块的大小，而无需手动干预（除非你关掉均衡器）。