---
title: linux环境搭建pytorch环境
date: 2023年3月28日10:44:17
tags: 
  - linux
  - pytorch
categories: 环境配置
---
用来配置服务器python环境的记录
{% cb 搭建doctor,false, false %} 
<!-- more -->


# 1安装mini-conda
[官网](https://docs.conda.io/en/latest/miniconda.html#linux-installers)
从官网下载对应版本，对应系统的安装包

将安装包传入linux系统
cd到目标文件目录：
```
bash Miniconda3-4.7.12.1-Linux-x86_64.sh
```

安装后重新加载配置文件
```
source .bashrc
```

# 2基础命令
```
conda --version 查看安装的conda的版本，检验是否安装成功
conda list：查看安装了哪些包。
conda search XXXX:查找包的版本
conda install package_name(包名)：安装包
conda env list 或 conda info -e：查看当前存在哪些虚拟环境
conda update conda：检查更新当前conda
conda create -n 创建conda环境
激活环境 conda activate XXX
退出环境conda deactivate
移除环境： conda/mamba remove -n XXX --all
导出环境： conda env export xxx.yaml 将现有的包导出，方便以后直接创建环境
复现环境： conda env create -f environment.yaml
lsblk查看硬盘信息
top 查看cpu使用情况
lspci | grep -i nvidia  查看gpu情况
```

添加镜像
```
conda config --add channels genomedk
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --set show_channel_urls yes

#查看已配置的镜像
conda config --show

#删除某个镜像源
conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```



# 3配置pytorch环境
查看CUDA版本
```
nvidia-smi
```
[pytorch官网](https://pytorch.org/get-started/locally)
选择对应的版本

判断是否安装成功
在命令行输入```python```

输入```import torch```，没有报错说明安装成功

输入```torch.cuda.is_available()```，返回true表示可以使用GPU

# 4安装jupyter
[源于](https://zhuanlan.zhihu.com/p/166425946)
jupyter notebook安装
```conda install jupyter notebook```

jupyter lab安装
```conda install -c conda-forge jupyterlab```

使用pip安装中文界面
```pip install jupyterlab-language-pack-zh-CN ``` 


生成jupyter密码密文
密文生成有手动生成和自动生成两种方式。自动生成会保存在json文件中。手动生成在ipython中运行直接输出。 密码用于本地访问服务器jupyter时需要输入的密码。

自动生成
```
$ jupyter notebook password
Enter password:  
Verify password: 
[NotebookPasswordApp] Wrote hashed password to /home/you/.jupyter/jupyter_notebook_config.json
```

手动生成
```
## 打开ipython，输入
from IPython.lib import passwd
passwd()
## 提示输入密码和确认输入，完成后得到密文。
Enter password: 
Verify password: 
'sha1:********************************************' 
## 或者
from notebook.auth import passwd; passwd()
```
配置

生成密文后，有两种设置方式，访问jupyter。一种是修改配置文件，另一种为通过ssh启动。

方式一：修改配置文件
查看配置文件是否存在：```~/.jupyter/jupyter_notebook_config.py```
生成配置文件：
```
jupyter notebook --generate-config
或
jupyter lab --generate-config
```

修改配置文件 需要修改主要是四个地方，分别是IP地址，密文，浏览器，端口。
```
## 打开前面生成的配置文件
vim ~/.jupyter/jupyter_notebook_config.py
## 修改配置内容
## 官方文档建议修改成‘*’，但可能还是无法访问，可以修改成‘0.0.0.1’或者服务器IP 目前不需改
c.NotebookApp.ip='*'
## 修改成将之前生成的密文 改shal:后面的内容
c.NotebookApp.password = u'sha1:........'
c.NotebookApp.open_browser = False
## 随便都可以，防止与其他端口冲突即可。也可以再启动jupyter时，用--port参数指定。
c.NotebookApp.port =8889
```

[jupyter lab插件](https://zhuanlan.zhihu.com/p/101070029)

服务器常用命令
```nohup jupyter notebook &```后台启动jupyter lab


```pkill -f jupyter```杀死后台所有jupyter进程

```htop```查看cpu占用情况

多内核jupyter

```conda install nb_conda -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/```

# 内网穿透
跳板机
B to C ```ssh -N -f -L 3333:localhost:8889 syf@172.19.4.27```


 开机自启 （有问题）
 您可以创建一个名为 my_script.sh 的文件，然后在其中添加以下内容：
 
 
```
#!/bin/bash
ssh -N -f -L 3333:localhost:8889 syf@172.19.4.27
```
然后，您可以使用以下命令将脚本移动到 /etc/profile.d/ 目录下：



```sudo mv my_script.sh /etc/profile.d/```

最后，您需要给脚本添加可执行权限：



```sudo chmod +x /etc/profile.d/my_script.sh```

这样，在系统启动后，脚本就会自动执行。

 
 
打洞
[cpolar](https://www.cpolar.com/)
使用

5. 向系统添加服务
```sudo systemctl enable cpolar```
ShellCopy
6. 启动cpolar服务
```sudo systemctl start cpolar```

