---
title: docker与docker-compose下载安装
date: 2025-07-10
categories:
  - 技术分享
tags: 
  - Docker
  - Docker Compose
location: 安徽 合肥
cover: https://s21.ax1x.com/2025/07/11/pVlFNwD.png
top_img: https://s21.ax1x.com/2025/07/11/pVlFNwD.png
copyright: enable #【可选】显示文章版权模块(默认为设置中 post_copyright 的 enable 配置)
copyright_author: Wizard小志 #【可选】文章版权模块的文章作者
copyright_author_href: https://yanruizhi.github.io/ #【可选】文章版权模块的文章作者的链接
# copyright_url: https://xxxxxx.com #【可选】文章版权模块的文章链接
copyright_info: 此文章版权归Wizard小志所有，如有转载，请注明来自原作者 # 其他描述信息
---

# docker与docker-compose下载安装

## 一、docker安装

### 1.1 yum准备

1. 更新yum

~~~bash\
yum update
~~~

2. 安装yum插件

~~~sh
yum install -y yum-utils device-mapper-persistent-data lvm2
~~~

3. 设置阿里云docker-ce镜像源

~~~sh
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
~~~

4. 安装docker

~~~sh
yum install -y docker-ce
~~~

5. 安装后处理

~~~sh
#启动docker命令
systemctl start docker
#设置开机自启命令
systemctl enable docker
#查看docker版本命令
docker version

#创建docker配置文件目录
mkdir -p /etc/docker 
#添加配置内容
tee /etc/docker/daemon.json <<-'EOF'
 {
  "registry-mirrors": [
    "https://docker.1panelproxy.com",
    "https://2m11665s.mirror.aliyuncs.com",
    "https://registry.docker-cn.com",
    "https://dockerhub.azk8s.cn",
    "https://docker.mirrors.ustc.edu.cn",
    "http://hub-mirror.c.163.com",
    "https://k8s.gcr.io",
    "https://github-releases.githubusercontent.com",
    "https://vsxcs7sq.mirror.aliyuncs.com",
    "https://ustc-edu-cn.mirror.aliyuncs.com"]
}
EOF
#重启docker
systemctl restart docker

~~~









## 二、 docker-compose安装

### 2.1 二进制文件安装

#### 2.1.1  github官网下载

1.下载二进制文件

```bash
curl -SL https://github.com/docker/compose/releases/download/v2.29.1/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
```

2.赋予文件执行权限

``` bash
sudo chmod +x /usr/local/bin/docker-compose
```

3.建立软连接

```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

4.查看是否安装成功

```bash
docker-compose --version
```

若显示`Docker Compose version v2.29.4`则表示安装成功

#### 2.1.2  DaoCloud国内源下载

1.下载文件

```bash
curl -L http://get.daocloud.io/docker/compose/releases/download/v2.17.2/docker-compose-`uname -s`-`uname -m` > /applications/docker-compose
curl -L https://g.nite07.org/docker/compose/releases/download/v2.24.7/docker-compose-`uname -s`-`uname -m` > /applications/docker-compose

```

### 2.2 使用pip安装。

```bash
$ pip install docker-compose
或者
pipx install docker-compose
```

### 2.3 使用wget下载

首先在github上找到需要的release版本`https://github.com/docker/compose/releases`,然后复制下载链接

[![复制下载链接](https://s21.ax1x.com/2025/07/11/pVQvjfK.md.png)](https://s21.ax1x.com/2025/07/11/pVQvjfK.png)

```bash
wget https://github.com/docker/compose/releases/download/v2.38.2/docker-compose-darwin-x86_64 > /applications/docker-compose
```

