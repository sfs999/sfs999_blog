---
title: 搭建服务器环境所使用到的一些应用与端口的记录
date: 2023年5月3日13:15:10
tags: 
  - 服务器
categories: 环境配置
---
用来配置服务器环境
<!-- more -->

# 常用应用端口：

## ssh：
```
tcp://127.0.0.1:22 

```

登录指令：
```
用户名@网址 -p 端口号

例：
song@2.tcp.cpolar.top -p 11048
```

## coplar web UI：
```
http://localhost:9200 

```

在coplar中开启内网穿透时可以选择只开启https，一方面保证安全性，另一方面可以多开一倍的隧道
[coplar下载地址](https://www.cpolar.com/download)
在win和mac平台直接就是web界面，在linux平台需要编辑配置文件来开启端口

## Portainer：
```
http://localhost:9000 

```

安装指令：
```
docker run -d \
-p 9000:9000 \
-p 8000:8000 \
--restart always \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /opt/docker/portainer-ce/data:/data \
--name portainer-ce portainer/portainer-ce
```

Portainer也可以实现远程控制其他docker节点

## clash：
```
http://localhost:9090 

```


控制面板网址：
```
clash-dashboard：http://clash.razord.top/
yacd-dashboard：http://yacd.haishan.me/
```

在控制面板中将clash的后台服务端口输入就可以访问了

[docker部署](https://blog.zzsqwq.cn/posts/how-to-use-clash-on-linux/)

## Jupyter lab：
```
http://localhost:8888 

```

远程访问Jupyter lab需要修改配置文件如下：

```
c.NotebookApp.allow_remote_access = True
c.NotebookApp.open_browser = False
c.NotebookApp.ip='*'
c.NotebookApp.password = 'sha1:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'

```

## chat_academic：
```
http://localhost:50923 

```

注意，使用docker部署时需要将localhost改为host.docker.internal才可以访问宿主机的端口