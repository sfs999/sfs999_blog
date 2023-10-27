---
title: 搭建本地GEE python环境的步骤
date: 2023年2月2日17:40:30
tags: 
  - GEE
  - GEE_learn
categories: 环境配置
---

使用jupyter notebook搭建，避免安装客户端等操作
最近要求学习GEE，由与想要部署本地的PY环境，在部署环境的过程中踩了一些坑，但参考大佬的文章最终成功部署了本地环境，以下为只需安装earthengine-api的部署方法
本文写于2022年12月12日，该方法实测有效
<!-- more -->
### 首先需要科学上网！！！

### 注册谷歌账号并申请GEE使用
关于注册谷歌账号的方法已经有很多了，这里给一个指路
本文参考: [GEE学习系列——GEE账号注册](https://blog.csdn.net/weixin_43536034/article/details/125326723?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167083736816782390539750%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167083736816782390539750&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-125326723-null-null.142^v68^control,201^v4^add_ask,213^v2^t3_control1&utm_term=GEE%E8%B4%A6%E5%8F%B7&spm=1018.2226.3001.4187)

### 之后需要本地环境安装earthengine-api
此处需要安装好python pip jupter，由于都是通用工具，所以本文不再介绍

在终端中输入：
```
pip install earthengine-api;
```
注意，pip安装包时要关闭代理

###  jupter在线编程的实现
这里参考这篇文章：
谷歌网盘参考: [GEE入门【1】| Python环境配置](https://blog.csdn.net/weixin_43360896/article/details/108174759?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167030206516800182159539%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167030206516800182159539&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-108174759-null-null.142^v67^wechat,201^v4^add_ask,213^v2^t3_control1&utm_term=gee%E9%85%8D%E7%BD%AEpython%E7%8E%AF%E5%A2%83&spm=1018.2226.3001.4187)

根据文章中的内容注册好谷歌网盘后再根据
![在这里插入图片描述](https://img-blog.csdnimg.cn/fc3fdeabd0fd4805bf8c3ba78e1d84e7.png#pic_center)
中的本地jupter连接方法
首先在终端中执行
```
pip install jupyter_http_over_ws
jupyter serverextension enable --py jupyter_http_over_ws
```
安装网页扩展包并确认jupter版本符合

之后在本地终端执行jupter
```
jupyter notebook --NotebookApp.allow_origin='https://colab.research.google.com' --port=8888 --NotebookApp.port_retries=0
```
在在线环境中连接本地环境

![在这里插入图片描述](https://img-blog.csdnimg.cn/f27f460d0eb244af8e67b9cb83643944.png#pic_center)

将jupter网址填入

![在这里插入图片描述](https://img-blog.csdnimg.cn/704053a29b58477894b3f89fa47c7761.png#pic_center)

之后执行代码：
```
import ee
ee.Authenticate()
```
如果出现这样则点击连接进行授权，可以避免谷歌客户端包等下载
![在这里插入图片描述](https://img-blog.csdnimg.cn/0cbd2a5b81d14566bb9a3801381d6215.png#pic_center)
将授权码填入后回车，此时该设备已经授权完成。

### 关于本地接口的设置
接口的设置参考: [【笔记】GEE之python学习](https://blog.csdn.net/awdwd233333/article/details/123394694?ops_request_misc=&request_id=&biz_id=102&utm_term=gee%20python&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-123394694.142^v67^wechat,201^v4^add_ask,213^v2^t3_control1&spm=1018.2226.3001.4187)

在本地的编译器交互窗口，或者jupter中完成GEE的初始化
```
#初始化本地GEE服务
import ee
import os
os.environ['HTTP_PROXY'] = 'http://127.0.0.1:1111'
os.environ['HTTPS_PROXY'] = 'http://127.0.0.1:1111'
ee.Initialize()
#print(ee.Image('USGS/SRTMGL1_003').getInfo())
```
这里'http://127.0.0.1:1111'中后四位为你电脑代理的端口，此处若有问题请自行百度

之后就可以完全本地连接GEE进行编程了