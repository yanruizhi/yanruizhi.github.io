---
title: CentOS7安装高版本node依赖库报错问题解决
date: 2025-07-17
categories:
  - 技术分享
tags: 
  - CentOS
  - node
location: 安徽 合肥
cover: https://s21.ax1x.com/2025/07/17/pV3VpnI.png
top_img: https://s21.ax1x.com/2025/07/17/pV3VpnI.png
copyright: enable #【可选】显示文章版权模块(默认为设置中 post_copyright 的 enable 配置)
copyright_author: Wizard小志 #【可选】文章版权模块的文章作者
copyright_author_href: https://yanruizhi.github.io/ #【可选】文章版权模块的文章作者的链接
# copyright_url: https://xxxxxx.com #【可选】文章版权模块的文章链接
copyright_info: 此文章版权归Wizard小志所有，如有转载，请注明来自原作者 # 其他描述信息
---

# CentOS7安装高版本node依赖库报错问题解决

### 1. 问题如下

当运行node和npm命令时报错缺少某些依赖库

[![pVGCnYQ.png](https://s21.ax1x.com/2025/07/22/pVGCnYQ.png)](https://imgse.com/i/pVGCnYQ)

问题是GLIBC文件未找到，网上搜索发现是版本问题

输入命令查看当前clibc版本信息

```bash
strings /lib64/libc.so.6 |grep GLIBC_
```

显示如下

```bash
GLIBC_2.2.5
GLIBC_2.2.6
GLIBC_2.3
GLIBC_2.3.2
GLIBC_2.3.3
GLIBC_2.3.4
GLIBC_2.4
GLIBC_2.5
GLIBC_2.6
GLIBC_2.7
GLIBC_2.8
GLIBC_2.9
GLIBC_2.10
GLIBC_2.11
GLIBC_2.12
GLIBC_2.13
GLIBC_2.14
GLIBC_2.15
GLIBC_2.16
GLIBC_2.17
GLIBC_PRIVATE
___sys_nerr_GLIBC_2_3
___sys_nerr_GLIBC_2_4
___sys_nerr_GLIBC_2_1
...
```

### 2. 更新GLIBC

首先下载高版本的glibc [点击下载glibc-2.28](http://ftp.gnu.org/gnu/glibc/glibc-2.28.tar.gz)

或者使用wget下载

```sh
wget http://ftp.gnu.org/gnu/glibc/glibc-2.28.tar.gz
```

然后解压并安装

```sh
tar xf glibc-2.28.tar.gz 
cd glibc-2.28/ && mkdir build  && cd build
../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
```

编译过程遇到gcc问题，说明版本过低

### 3. 更新gcc

[下载gcc包]([https://ftp.gnu.org/gnu/gcc](https://links.jianshu.com/go?to=https%3A%2F%2Fftp.gnu.org%2Fgnu%2Fgcc))

这里选用9.1.0版本的

解压

```sh
tar -zvxf gcc-9.10.tar.gz --directory=/usr/local/
```

找到gcc目录下**contrib**目录中的**download_prerequisites**文件，查看内容

[![pVGCMSs.png](https://s21.ax1x.com/2025/07/22/pVGCMSs.png)](https://imgse.com/i/pVGCMSs)

下载上述的几个包并解压到gcc目录下

[gmp-6.1.0.tar.bz2](https://ftp.gnu.org/pub/gnu/gmp/gmp-6.1.0.tar.bz2)
[mpfr-3.1.4.tar.bz2](http://mirror.hust.edu.cn/gnu/mpfr/mpfr-3.1.4.tar.bz2)
[mpc-1.0.3.tar.gz](https://ftp.gnu.org/gnu/mpc/mpc-1.0.3.tar.gz)
[isl-0.18.tar.bz2](http://www.mirrorservice.org/sites/sourceware.org/pub/gcc/infrastructure/isl-0.18.tar.bz2)

```sh
tar -xf gmp-6.1.0.tar.bz2
tar -xf mpfr-3.1.4.tar.bz2
tar -xf mpc-1.0.3.tar.gz
tar -xf isl-0.18.tar.bz2
```

建立软连接

```sh
ln -sf gmp-6.1.0 gmp
ln -sf mpfr-3.1.4 mpfr 
ln -sf mpc-1.0.3 mpc
ln -sf isl-0.18 isl
```

升级gcc（两个小时左右）

```sh
# 创建build目录
mkdir build && cd build
# 配置
../configure -enable-checking=release -enable-languages=c,c++ -disable-multilib
# 安装
make && make install
```

完成后继续执行第一步编译，又出现下面问题

[![pVGCQln.png](https://s21.ax1x.com/2025/07/22/pVGCQln.png)](https://imgse.com/i/pVGCQln)

显示make版本太老旧

### 4. 更新make

[下载make4.3](http://ftp.gnu.org/gnu/make/make-4.3.tar.gz)

或者使用wget下载

```sh

# 升级 make(默认为3 升级为4)
wget http://ftp.gnu.org/gnu/make/make-4.3.tar.gz
# 解压
tar -xzvf make-4.3.tar.gz && cd make-4.3/
# 执行安装
./configure  --prefix=/usr/local/make
make && make install
cd /usr/bin/ && mv make make.bak
ln -sv /usr/local/make/bin/make /usr/bin/make
```

完成后执行**make -v**

[![pVGCGwT.png](https://s21.ax1x.com/2025/07/22/pVGCGwT.png)](https://imgse.com/i/pVGCGwT)

### 5.  CXXABI问题

然后继续编译glibc

```sh
cd glibc-2.28/build
../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
```

成功后执行安装命令（时间较长）

```sh
make && make install
```

完成后执行**node -v**，发现还是有问题

[![pVGCJTU.png](https://s21.ax1x.com/2025/07/22/pVGCJTU.png)](https://imgse.com/i/pVGCJTU)

查看CXXABI

```sh
strings /usr/lib64/libstdc++.so.6 | grep CXXABI
```

更新libstdc++.so.6

```sh
# 下载libstdc++.so.6
wget https://cdn.frostbelt.cn/software/libstdc%2B%2B.so.6.0.26
# 如果不行用下面这个
wget --no-check-certificate https://cdn.frostbelt.cn/software/libstdc%2B%2B.so.6.0.26
# 替换系统中的/usr/lib64
cp libstdc++.so.6.0.26 /usr/lib64/
cd /usr/lib64/ && ln -snf ./libstdc++.so.6.0.26 libstdc++.so.6
```

### 6.  大功告成

``` sh
[root@localhost lib64]# node -v
v22.17.0
```





