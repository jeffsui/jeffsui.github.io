title: "how to build a private docker registry"
date: 2015-07-21 23:09:19
category: docker
tags: [docker,registry,registry-ui]
---

# 如何搭建docker私服

##环境准备

软件包：

* centos6.5_x86_64

* docker-engine-1.7.0-1.el6.x86_64.rpm

docker环境搭建,请参照官方说明，本文采用的是官方的rpm包

##何谓私服

官方的image镜像站位dockerhub,因为伟大的墙的缘故,所以下载镜像是很痛苦的一件事。当然你可以采用其他科学上网或者镜像加速的方法来获取image。
docker官方也提供了一个私服镜像,大家可以通过`docker search registry`来查找该镜像。

```
NAME                                     DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
registry                                 Containerized docker registry                   320       [OK]       
atcol/docker-registry-ui                 A web UI for easy private/local Docker Reg...   55                   [OK]
konradkleine/docker-registry-frontend    Browse and modify your Docker registry in ...   40                   [OK]
samalba/docker-registry                                                                  35 
```

##下载官方registry镜像

* 下载镜像

使用命令`docker pull registry`执行下载镜像。

* 查看镜像

下载完毕后,通过`docker images` 查看该镜像。

* 给镜像打标签

执行这个命令
`docker tag registry:latest localhost:5000/registry:latest`


##启动镜像 

`docker run -d -e SETTINGS_FLAVOR=dev -e STORAGE_PATH=/tmp/registry -v /opt/data/registry:/tmp/registry  -p 5000:5000 registry`

这里有几个参数说明下:
* 1.`-e STORAGE_PATH=/tmp/registry` :强制使用存储路径
* 2.`-v /opt/data/registry:/tmp/registry` :绑定本地镜像存储路径
* 3.`-p 5000:5000`:映射容器5000端口到本地5000端口

##查看镜像状态

`docker ps`

##查看私服状态

`curl http://localhost:5000`

显示如下信息,证明registry启动成功:

`"\"docker-registry server\""`

##推送本地镜像库到registry私服

###1. 第一步 给本地镜像 打tag
例如给官方的nginx镜像打tag,执行下面的命令行
`docker pull nginx`
`docker tag nginx:latest localhost:5000/nginx:latest`
查看镜像库,发现`localhost:5000/nginx`的镜像已经有了。


###2. 第二步 推送tag到registry私服

`docker push localhost:5000/nginx:latest `


###3. 第三步 查看私服镜像列表

`curl http://localhost：5000/V1/search`

看到类似这样的信息

```

{"num_results": 5, "query": "", "results": [{"description": null, "name": "correl/erlang"}, {"description": null, "name": "linfeng/cmd"}, {"description": null, "name": "library/my_nodejs_image"}, {"description": null, "name": "library/centos"}, {"description": "", "name": "library/nginx"}]}

```
##拉取私服镜像

`docker pull 192.168.20.85:5000/library/centos:7 `

##结论

这只是演示如何搭建一个简单的registry私服。因为只有通过命令行方式才能查看私服信息,所以不是很便于操作。下面的博文将演示如何给registry添加web界面。

