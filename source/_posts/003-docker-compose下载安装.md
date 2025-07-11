---
title: docker-compose下载安装
date: 2025-07-10
categories:
  - 技术分享
tags: 
  - docker
location: 安徽 合肥
cover: https://s21.ax1x.com/2025/07/11/pVlFNwD.png
top_img: https://s21.ax1x.com/2025/07/11/pVlFNwD.png
copyright: enable #【可选】显示文章版权模块(默认为设置中 post_copyright 的 enable 配置)
copyright_author: Wizard小志 #【可选】文章版权模块的文章作者
copyright_author_href: https://yanruizhi.github.io/ #【可选】文章版权模块的文章作者的链接
# copyright_url: https://xxxxxx.com #【可选】文章版权模块的文章链接
copyright_info: 此文章版权归Wizard小志所有，如有转载，请注明来自原作者 # 其他描述信息
---

# docker-compose下载安装

## 一、二进制文件安装

### ① github官网下载

#### 1.下载二进制文件

```bash
curl -SL https://github.com/docker/compose/releases/download/v2.29.1/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
```

#### 2.赋予文件执行权限

``` bash
sudo chmod +x /usr/local/bin/docker-compose
```

#### 3.建立软连接

```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

#### 4.查看是否安装成功

```bash
docker-compose --version
```

若显示`Docker Compose version v2.29.4`则表示安装成功

### ② DaoCloud国内源下载

#### 1.下载文件

```bash
curl -L http://get.daocloud.io/docker/compose/releases/download/v2.17.2/docker-compose-`uname -s`-`uname -m` > /applications/docker-compose
curl -L https://g.nite07.org/docker/compose/releases/download/v2.24.7/docker-compose-`uname -s`-`uname -m` > /applications/docker-compose

```

## 二、使用pip安装。

```bash
$ pip install docker-compose
或者
pipx install docker-compose
```

## 三、使用wget下载

首先在github上找到需要的release版本`https://github.com/docker/compose/releases`,然后复制下载链接

[![复制下载链接](https://s21.ax1x.com/2025/07/11/pVQvjfK.md.png)](https://imgse.com/i/pVQvjfK)

```bash
wget https://github.com/docker/compose/releases/download/v2.38.2/docker-compose-darwin-x86_64 > /applications/docker-compose
```

