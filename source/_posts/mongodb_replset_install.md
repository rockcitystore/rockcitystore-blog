---
title: mongodb replset 部署记录
date: 2017-06-17 15:00:00
updated: 2017-06-17 15:00:00
---


## 安装
[Install MongoDB Community Edition From Tarball](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-linux/)


#### 解压 配置储存目录
```
mkdir -p /opt/logs/mongodb/ /opt/mongodb_store /opt/software && cd /opt/software/ && tar -zxvf mongodb-linux-x86_64-3.4.5.tgz
```
#### 添加开机执行
在 `/etc/rc.local`增加一行
```
# This script will be executed *after* all the other init scripts.  
# You can put your own initialization stuff in here if you don't  
# want to do the full Sys V style init stuff. 
 
 # mongod_start
bash /opt/software/mongod_start
```

#### 启动脚本内容mongod_start
```
#! /bin/bash  
mongod --dbpath /opt/mongodb_store --replSet ads_mongodb_repl --logpath /opt/logs/mongodb/mongod.log --fork --auth --keyFile /opt/software/mongodb_keyFile
#mongod --config /opt/software/mongod.conf
```
#### 将启动脚步放在`/opt/software/mongod_start`

#### 配置环境变量

```
在`/etc/profile`增加一行`export PATH=/opt/software/mongodb-linux-x86_64-3.4.5/bin/:$PATH`
重登ssh
```




## 权限配置
#### 生产key
在219上
```
openssl rand -base64 756 > /opt/software/mongodb_keyFile
chmod 400 /opt/software/mongodb_keyFile
scp -r mongodb_keyFile root@10.37.134.220:/opt/software/
scp -r mongodb_keyFile root@10.37.134.221:/opt/software/
```
#### 添加用户
在 PRIMARY 执行 mongo
```
// root
db.getSiblingDB("admin").createUser({user:"root", pwd: "JUnjkad1|>L", roles: ["root"]})

// admin
db.getSiblingDB("admin").createUser({user: "admin",pwd: "JKSXD:?%!@#",roles: [ { role: "userAdminAnyDatabase", db: "admin" }]})
db.getSiblingDB("admin").auth("admin", "JKSXD:?%!@#" )

//biz
db.getSiblingDB("ads").createUser({user:"ads", pwd:"LN<>.G*&!@|", roles:["readWrite", "dbAdmin"]})
db.getSiblingDB("ads").auth("ads", "LN<>.G*&!@|" )

//test
db.getSiblingDB("ads_test").createUser({user:"ads_test", pwd:"LN<>.G*&!@|", roles:["readWrite", "dbAdmin"]})
db.getSiblingDB("ads_test").auth("ads_test", "LN<>.G*&!@|" )
```

#### ads 登录
mongo -u ads -p LN\<\>\.G\*\&\!\@\| --authenticationDatabase "ads"
#### admin 登录
mongo -u admin -p JKSXD\:\?\%\!\@\# --authenticationDatabase "admin"
#### root 登录
mongo -u root -p JUnjkad1\|\>\L --authenticationDatabase "admin"




## 集群配置
[集群配置](https://www.mtyun.com/library/43/MongoDB-high-availability/)
http://www.jianshu.com/p/2a06f7f81814
#### 初始化集群
在219上执行 随后自提升为PRIMARY

```
rs.initiate({
_id:"ads_mongodb_repl",
 members : [ 
   {_id : 1, host : "10.37.134.219:27017"},
   {_id : 2, host : "10.37.134.220:27017"}, 
   {_id : 3, host : "10.37.134.221:27017"}, 
   ]  
   });
```


## 初始化
#### 添加collection
`db.createCollection("project")`
`db.createCollection("user")`



## 常用命令


#### start
`#sudo service mongod start`
`bash /opt/software/mongod_start`

#### 查看进程
`ps -ef | grep mongo | grep -v grep`

#### config
`vi /etc/mongod.conf`
#### log
`tail -f /var/log/mongodb/mongod.log`
#### port
`27017`

#### restart
`sudo service mongod restart`

#### stop
`sudo service mongod stop`

#### 编辑索引
`db.collection.reIndex()`
`sudo service mongod restart`

