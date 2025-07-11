---
title: 一键部署chevereto图床
date: 2025-07-11
categories:
  - 技术分享
tags: 
  - docker
  - chevereto
  - 图床
location: 安徽 合肥
cover: https://pic4.zhimg.com/v2-9a946f76b65c678a3125b8f42aba9984_1440w.jpg?source=172ae18b
top_img: https://pic4.zhimg.com/v2-9a946f76b65c678a3125b8f42aba9984_1440w.jpg?source=172ae18b
copyright: enable #【可选】显示文章版权模块(默认为设置中 post_copyright 的 enable 配置)
copyright_author: Wizard小志 #【可选】文章版权模块的文章作者
copyright_author_href: https://yanruizhi.github.io/ #【可选】文章版权模块的文章作者的链接
# copyright_url: https://xxxxxx.com #【可选】文章版权模块的文章链接
copyright_info: 此文章版权归Wizard小志所有，如有转载，请注明来自原作者 # 其他描述信息
---

# chevereto 个人图床的部署

### 1.前言

问题很简单，就是写完博客发现图片都打不开，为了解决问题，网上看了一些帖子，也试了一下其他图床，但感觉免费版效果还是不太好，还有就是上传下载限制问题。

于是打算自己部署一套图床程序，网上了解到chevereto是一个比较不错的图床解决方案，于是尝试部署一下。

### 2.部署

#### 2.1 访问dockerHub查询镜像

推荐使用docker方式部署，因为这样会省去其他插件的部署，一条命令就OK了

首先进入dockerHub网站搜索chevereto 项目（如果打不开请科学上网），第一个就是官方版本，更新频率很高

![dockerhub搜索](https://s21.ax1x.com/2025/07/11/pVlFSeS.png)

点击进去会看到相应的docker命令

我们使用第一个命令就好

```shell
docker run -d \
  --name chevereto \
  -p 80:80 \
  -e CHEVERETO_DB_HOST=database \ #数据库ip或主机地址
  -e CHEVERETO_DB_USER=chevereto \ #数据库用户名
  -e CHEVERETO_DB_PASS=user_database_password \ #数据库密码
  -e CHEVERETO_DB_PORT=3306 \ #数据库端口
  -e CHEVERETO_DB_NAME=chevereto \ #数据库名
  -e CHEVERETO_MAX_POST_SIZE=2G \ #最大上传大小
  -e CHEVERETO_MAX_UPLOAD_SIZE=2G \ #最大下载大小
  -v /var/www/html/images/ \ #目录挂载
  chevereto/chevereto:latest 

```

#### 2.2 数据库的创建

在运行命令前先创建对应的数据库，否则会报错`SQLSTATE[HY000] [1049] Unknown database 'chevereto'`。上述默认`chevereto`

#### 2.3 运行命令

运行命令后待程序启动，访问`http://ip:port`

如果遇到下面问题，则说明挂载的目录没有写入权限，`chmod +777 目录`加一下权限即可

![报错](https://s21.ax1x.com/2025/07/11/pVlF9oQ.png)

#### 2.4 页面配置

创建管理员用户

然后登录![仪表盘](https://s21.ax1x.com/2025/07/11/pVlFPij.png)

点击中间图标下的Settings按钮设置语言设置成中文即可

![设置](https://s21.ax1x.com/2025/07/11/pVlFiJs.png)

