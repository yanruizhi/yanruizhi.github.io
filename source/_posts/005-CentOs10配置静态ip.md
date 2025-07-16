---
title: CentOS Stream 10配置静态ip
date: 2025-07-16
categories:
  - 技术分享
tags: 
  - CentOS
location: 安徽 合肥
cover: https://pic4.zhimg.com/v2-9a946f76b65c678a3125b8f42aba9984_1440w.jpg?source=172ae18b
top_img: https://pic4.zhimg.com/v2-9a946f76b65c678a3125b8f42aba9984_1440w.jpg?source=172ae18b
copyright: enable #【可选】显示文章版权模块(默认为设置中 post_copyright 的 enable 配置)
copyright_author: Wizard小志 #【可选】文章版权模块的文章作者
copyright_author_href: https://yanruizhi.github.io/ #【可选】文章版权模块的文章作者的链接
# copyright_url: https://xxxxxx.com #【可选】文章版权模块的文章链接
copyright_info: 此文章版权归Wizard小志所有，如有转载，请注明来自原作者 # 其他描述信息
---

# CentOs Stream 10 配置静态ip

CentOs10与CentOs7的配置文件方面有很大的区别，可以说完全不一样了，可见10在以往版本的基础上做了重大改动。

官方不推荐把CentOs10用作生产环境，因为这是一个一直维护更新的版本，就像windows时常需要更新一样。

### 1. 打开网卡配置文件

CentOS Stream 10 的网卡配置文件在 `/etc/NetworkManager/system-connections/` 目录下，打开对应的网卡配置文件：

```sh
vim /etc/NetworkManager/system-connections/ens33.nmconnection
```


如果觉得没把握最好先备份上述文件。

**ens33.nmconnection**配置如下

```tex
[root@centos10model system-connections]# cat ens33.nmconnection.bak
[connection]
id=ens33
uuid=bc666755-b111-30f0-88e6-199803e48aa6
type=ethernet
autoconnect-priority=-999
interface-name=ens33
timestamp=1742935328

[ethernet]

[ipv4]
method=auto

[ipv6]
addr-gen-mode=eui64
method=auto

[proxy]

```

基本上只有ipv4要修改

```tex
[root@centos10model system-connections]# cat ens33.nmconnection
[connection]
id=ens33        #网卡id
uuid=bc666755-b111-30f0-88e6-199803e48aa6  ##uuid
type=ethernet                    #网卡
autoconnect-priority=-999       #不知道这个干什么的，应该是开机启动
interface-name=ens33           #网卡名字
timestamp=1742915250            #linux的时间戳

[ethernet]

[ipv4]
address1=192.168.230.130/24     #ip/掩码
gateway=192.168.230.2           #网关          
method=manual               ## 改成manual(意思是设置手动模式)，auto自动获取

[ipv6]
addr-gen-mode=eui64     #ipv6，看情况，需要还是关闭
method=ignore            #auto自动获取，ignore关闭ipv6

[proxy]     #设置代理，这个我没设置

```

### 2. 重启服务

```sh
sudo systemctl restart NetworkManager
[root@localhost ~]# sudo nmcli c reload        # 重新加载配置文件
[root@localhost ~]# sudo nmcli c up ens33     # 重启ens33网卡
连接已成功激活（D-Bus 活动路径：/org/freedesktop/NetworkManager/ActiveConnection

```

### 3. nmcli 

也可以使用nmcli命令来修改网络配置（nmcli是NetworkManager客户端，CentOs10的网络管理工具就是NetworkManager）

```bash
查看运行状态：systemctl status NetworkManager
启动：systemctl start NetworkManager
重启：systemctl restart NetworkManager
关闭：systemctl stop NetworkManager
查看是否开机启动：systemctl is-enabled NetworkManager
开机启动：systemctl enable NetworkManager
禁止开机启动：systemctl disable NetworkManager
```

```bash
sudo nmcli con mod "ens33" ipv4.addresses 192.168.150.145/24 # IP地址,自己设置,要匹配网络范围
sudo nmcli con mod "ens33" ipv4.gateway 192.168.150.2		 # 网关,要和VMware中配置的虚拟网卡一致
sudo nmcli con mod "ens33" ipv4.dns "192.168.150.2"			 # DNS服务器，和网关一致即可 8.8.8.8 也可以
sudo nmcli con mod "ens33" ipv4.method manual				 # IPv4配置方法设置为手动,也就是启用静态IP配置
sudo nmcli con up "ens33"		# 使配置生效

添加第二个IP地址  nmcli con mod CON-NAME +ipv4.addresses 10.10.10.10/16
删除第二个IP地址  nmcli con mod CON-NAME -ipv4.addresses 10.10.10.10/16
添加dns1   nmcli con mod CON-NAME ipv4.dns 114.114.114.114
添加dns2   nmcli con mod CON-NAME +ipv4.dns  8.8.8.8


nmcli c m ens33 ipv4.address 192.168.80.10/24  # 修改 IP 地址和子网掩码
nmcli c m ens33 ipv4.method manual             # 修改为静态配置，默认是 auto
nmcli c m ens33 ipv4.gateway 192.168.80.2      # 修改默认网关
nmcli c m ens33 ipv4.dns 192.168.80.2          # 修改 DNS
nmcli c m ens33 +ipv4.dns 114.114.114.114      # 添加一个 DNS
nmcli c m ens33 ipv6.method disabled           # 将 IPv6 禁用
nmcli c m ens33 connection.autoconnect yes     # 开机启动
```

