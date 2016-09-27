title: how to use nginx on windows
date: 2016-09-27 10:27:56
category: nginx
tags: [nginx,windows]
---
# how to use nginx on windows
## 1. Download nginx lastest release from [here](http://nginx.org/en/download.html).

## 2. unzip to your local driver. eg: `c:/apps/nginx`

## 3. start nginx 
```
cd c:/apps/nginx
start nginx
```

## 4. monitoring nginx process
```
tasklist /fi "imagename eq nginx.exe"
Image Name                     PID Session Name        Session#    Mem Usage
========================= ======== ================ =========== ============
nginx.exe                    11700 Console                    1     10,696 K
nginx.exe                     1160 Console                    1     11,180 K
```

**notice**
一个是主进程(main process),另一个是工作进程(work process).如果启动失败,请查看错误日志`logs\error.log`

## 5. visit http://localhost:8080

## 6. configuration file `nginx.conf `
*reference config*
```
error_log  logs/error.log;
http {
    include       mime.types;
    default_type  application/octet-stream;
    server {
                listen       8080;
                server_name  localhost;
                location / {
                    root   html;
                    index  index.html index.htm;
                }
                error_page   500 502 503 504  /50x.html;
                location = /50x.html {
                    root   html;
                }
        }
    ...
}
```

## 7. the command list of nginx：
```
nginx -s stop   快速退出
nginx -s quit   优雅退出
nginx -s reload 更换配置，启动新的工作进程，优雅的关闭以往的工作进程
nginx -s reopen 重新打开日志文件
```
