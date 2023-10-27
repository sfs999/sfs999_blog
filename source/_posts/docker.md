---
title: linux环境搭建docker和portainer
date: 2023年2月8日17:40:44
tags: 
  - docker
  - portainer
categories: 环境配置
---
目前进度是开机启动portainer服务，然后局域网内的计算机可以访问网址操作portainer
接下来的问题：
{% cb 必要的时候的远程桌面,false, false %}  ：Windows自带，但需要一个HDMI诱骗器
{% cb wifi模块：外置天线,false, false %}
{% cb 修改待机时间,false, false %}

<!-- more -->

搭建过程：

# 1docker

```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world
```
Docker 安装完成，请检查是否出现 Hello from Docker!'

# 2portainer
```
sudo service docker restart

sudo docker volume create portainer_data
sudo docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
--restart=always \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainer_data:/data \
portainer/portainer-ce:latest

sudo docker ps
```
echo 'Portainer 安装完成，请检查是否出现 portainer/portainer-ce:latest 字样'
echo 'Web页面访问 https://127.0.0.1:9443/'
echo '或 https://服务器IP地址:9443/'