title: "windows-mongodb-install"
date: 2016-02-13 19:27:09
category: db
tags: [mongodb,windows]
---
#windows下mongodb安装

##下载mongodb

http://www.mongodb.org/downloads

##选择自定义安装

本机路径为:`d:\tools\mongodb`

##建立如下文件目录
数据库路径:`d:\tools\mongodb\db`
日志路径:`d:\tools\mongodb\log`
配置文件目录`d:\tools\mongodb\etc`
建立配置文件`d:\tools\mongodb\etc\mongodb.conf`

```
dbpath=d:\tools\mongodb\db #数据库路径
logpath=d:\tools\mongodb\log\mongodb.log #日志输出文件路径
logappend=true #错误日志采用追加模式，配置这个选项后mongodb的日志会追加到现有的日志文件，而不是从新创建一个新文件
journal=true #启用日志文件，默认启用
quiet=true #这个选项可以过滤掉一些无用的日志信息，若需要调试使用请设置为false
port=27017 #端口号 默认为27017
```
##启动服务

切换到`d:\tools\mongodb\bin` 目录下:

* 普通启动

`mongod --config d:\tools\mongodb\etc\mongodb.conf`

* 注册为windows服务

`mongod --config d:\tools\mongodb\etc\mongodb.conf --install`

**补充** 

* windows服务卸载

`mongod --remove --serviceName "MongoDB"`

* 启动服务 
`net start mongodb`

启动成功后,通过浏览器访问 `http://localhost:27017` ,看到下面的文字,证明启动服务成功！

> It looks like you are trying to access MongoDB over HTTP on the native driver port.

* 关闭服务
`net stop mongodb` 

#图形化工具

官方提供的很全:https://docs.mongodb.org/ecosystem/tools/administration-interfaces/

* [mongo express ](http://www.npmjs.org/package/mongo-express) --Nodejs

* [MongoBooster ](http://mongobooster.com)

* [UMongo ](http://edgytech.com/umongo/)

* [MongoHub ](https://github.com/fotonauts/MongoHub-Mac)

* [MongoVUE](http://blog.mongovue.com/) --.NET 







