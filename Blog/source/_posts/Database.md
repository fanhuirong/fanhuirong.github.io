# Database 

## Nosql

### MongoDB

### Couchbase


## 分布式数据库
### Master-Slave

1.	Setup a Master-Slave cluster: Including  
```bash
###
# 连接到某个节点的主机 一个session连一个节点
ssh test@mgrongslc01-6789.slc07.dev.ebayc3.com
ssh test@mgrongslc02-5802.slc07.dev.ebayc3.com 
ssh test@mgrongslc03-8805.slc07.dev.ebayc3.com 

# 1)安装 Software(binary) setup
sudo yum install mongodb-ebay-3.0.12 -y;sudo yum install mongodb-tools –y 


# 2)Configure the server
# 连接server节点 该节点用于管理信息
ssh stack@devbatch-5210.lvs01.dev.ebayc3.com 
# 无密码连接
ssh -i ~/.ssh/id_dev stack@devbatch-5210.lvs01.dev.ebayc3.com 

# 连接数据库
sudo  sqlite3 /export/home/oracle/bin/mongo.db；

# 插入数据 主机名 cluster名 接口  对每个host都执行该操作
 insert into mongod_conf (host,replica_set_name,interface, wiredtiger) values ('mgrongslc02-5802.slc07.dev.ebayc3.com',' RongRS',1,1);

# 3)start instance 在每个子节点里启动数据库
sudo start mongod interface=1

# 4)build them into a cluster
# 在某个子节点上 连接数据库并初始化 这个节点就是primary节点
  mongo `hostname` 
  rs.initiate() 
# 将其他两个端口加入进来变成secondary节点 rs.add('host:port')
  rs.add('mgrongslc03-8805.slc07.dev.ebayc3.com:27017')

  rs.help()
  rs.status() #查看状态
# 在primary节点上写的东西在其他节点上能够看到，但是在其他节点上无法进行写操作

```
2. Read/write some data with MongoDB in MongoDB shell
```bash
# 创建/切换数据库
use dbtest
show dbs
# 向集合插入数据 insert
i = { name : "Kven" }
k = { price : 180 }
db.people.insert( i )
db.breads.insert( k )
show collections

# update
db.people.update( {"name":"Kven"}, {"name":"John"} )

# find
db.people.find()

# delete
db.people.remove( {"name":"John"} )

```
[mongo-shell](https://docs.mongodb.com/manual/reference/mongo-shell/)

### capped collection


### isDal






