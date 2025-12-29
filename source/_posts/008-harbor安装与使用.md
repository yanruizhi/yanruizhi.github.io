---
title: 镜像仓库Harbor的安装与使用
date: 2025-12-29
categories:
  - 技术分享
tags: 
  - Harbor
  - Docker
location: 安徽 合肥
cover: https://s41.ax1x.com/2025/12/29/pZt96eO.jpg
top_img: https://s41.ax1x.com/2025/12/29/pZt96eO.jpg
copyright: enable #【可选】显示文章版权模块(默认为设置中 post_copyright 的 enable 配置)
copyright_author: Wizard #【可选】文章版权模块的文章作者
copyright_author_href: https://yanruizhi.github.io/ #【可选】文章版权模块的文章作者的链接
# copyright_url: https://blog.csdn.net/programmer_editor/article/details/123378329?utm_source=iteye #【可选】文章版权模块的文章链接
# copyright_info: 文章转载自CSDN作者《新程序员》编辑部 # 其他描述信息
---


# 私有镜像仓库Harbor的安装与使用

### 1. docker与docker-compose安装（前提）

具体安装方法见另一篇博客（[博客链接](https://yanruizhi.github.io/2025/07/10/003-docker-compose下载安装/)）

### 2.下载安装Harbor

如果wget下载较慢，也可自行下载再上传

`wget https://storage.googleapis.com/harbor-releases/harbor-offline-installer-v1.5.3.tgz`

下载完成后解压文件

`tar zxvf harbor-offline-installer-v1.5.3.tgz`

解压后harbor文件夹如下

![image-20251229141419284](https://s41.ax1x.com/2025/12/29/pZtCUnf.png)

移动harbor文件夹到/opt目录下

`mv harbor /opt`

修改配置文件 **harbor.yml**

`vi /opt/harbor/harbor.yml`

![image-20251229142749079](https://s41.ax1x.com/2025/12/29/pZtCaB8.png)

改一下主机地址和访问端口即可

如有需要也可改一下admin账户的密码`harbor_admin_password`,不改的话默认`Harbor12345`

修改完成后harbor根目录执行 `./prepare`命令和`./install.sh`进行安装

如果中途配置文件配置错了需要重新启动harbor，则先切换到harbor根目录，停止服务`docker-compose down`,然后修改配置，修改后执行`./prepare`,然后再启动harbor `docker-compose up -d`即可



### 3.UI界面

安装成功后访问之前配置的地址 `192.168.100.130:8089`

![image-20251229143644283](https://s41.ax1x.com/2025/12/29/pZt9Xfs.png)

![image-20251229143706686](https://s41.ax1x.com/2025/12/29/pZt9xlq.png)

### 4. 上传镜像

点进去一个项目（test是我自己创建的），点击推送命令，复制命令执行即可

![image-20251229143846020](https://s41.ax1x.com/2025/12/29/pZtCdHS.png)

如果仓库不是公开的需要提前登录，执行`docker login 192.168.100.130:8089`,在输入用户名密码即可

推送成功后如下![image-20251229144040732](https://s41.ax1x.com/2025/12/29/pZtC0Ag.png)
