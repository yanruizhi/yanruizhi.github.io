---
title: Docker可视化工具-portainer的使用与安装
date: 2026-11-20
categories:
  - 运维
tags: 
  - Docker
location: 安徽 合肥
cover: https://s41.ax1x.com/2026/11/23/pnLqR9P.png
top_img: https://s41.ax1x.com/2026/11/23/pnLqR9P.png
copyright: enable #【可选】显示文章版权模块(默认为设置中 post_copyright 的 enable 配置)
copyright_author: Wizard #【可选】文章版权模块的文章作者
copyright_author_href: https://yanruizhi.github.io/ #【可选】文章版权模块的文章作者的链接
# copyright_url: https://blog.csdn.net/programmer_editor/article/details/123378329?utm_source=iteye #【可选】文章版权模块的文章链接
# copyright_info: 文章转载自CSDN作者《新程序员》编辑部 # 其他描述信息
---



## （一）docker方式安装

1. **拉去镜像**

```Shell
# 查看镜像
docker search portainer
# 拉取镜像
docker pull portainer/portainer
#查看下载的本地镜像
docker images
#查看portainer详细信息
docker inspect portainer/portainer
```

1. **创建文件挂载**

```Shell
mkdir -p /applications/tools/portainer/data
```

1. **创建并运行容器**

```Shell
docker run -d \
-p 19000:9000 \
--name portainer \
--restart=always \
--privileged=true \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainer_data:/data \
portainer/portainer
```

参数解释

```Shell
docker run -d \
-p 9000:9000 \
--name portainer 
--restart=always \  # 是否开机启动
--privileged=true \
-v /var/run/docker.sock:/var/run/docker.sock \  # 配置文件挂载，这个不能自行更改，不然后面创建不了账户
-v portainer_data:/data \  # 数据存放的位置，一般是账户信息
portainer/portainer
```

1. 浏览器访问 **虚拟机ip+9000**端口（现在是英文的）(默认账号:

![image.png](https://s41.ax1x.com/2026/11/23/pnLqtt1.png)

1. **中文插件**

    第一步：下载文件\

    [public.zip](docker可视化工具-portainer+eae193d0-0ac0-459c-87d2-aaafffde151f/public.zip)

    第二步：将文件上传至**/home/portainer/**文件夹内并进行解压

    ```Shell
    unzip public.zip  # 如果显示没有命令可以先解压再上传，或者下载插件
    ```

此时的文件如下

![image.png](https://s41.ax1x.com/2026/11/23/pnLq0XD.png)

如果没有创建容器，这里可以一次性创建，这次比上次多挂载一个卷

```Shell
docker run -d \
-p 9000:9000 \
--name portainer \
--restart=always \
--privileged=true \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /applications/portainer/data:/data \
-v /applications/portainer/public:/public \
portainer/portainer

/home/docker/plugins/portainer/
```

之后访问ip + 9000即可

![image.png](https://s41.ax1x.com/2026/11/23/pnLqNfx.png)

