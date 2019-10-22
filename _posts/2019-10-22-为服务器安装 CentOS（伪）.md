---
layout: post
title: '为服务器安装 CentOS（伪）'
subtitle: '全是网络配置'
date: 2019-10-22
categories: 笔记 Linux
cover: '../assets/img/install-centos-7-logo.png'
tags: 笔记 Linux
---

# 为服务器安装 CentOS（伪）

之前用的都是云服务，这次尝试了一下手动安装 CentOS，之所以说是`伪`，是因为并没有在真正的物理服务器上安装，而是在虚拟机安装。

环境如下：

``` 
Windows 10 专业版 1903
VirtualBox 6
CentOS 7 (x86_64)
```

> VirtualBox 和 CentOS 均从清华大学镜像站下载
>
> https://mirrors.tuna.tsinghua.edu.cn/

安装过程没什么可说的，一路 next 和 continue，甚至可以选择中文界面，注意记住 root 密码和设置用户名以及设置好时区。

**在 VirtualBox 中注意网络的设置，这里会有坑，我使用的是桥接模式**

安装完成后，会提示重启，重启后输入用户名密码，就进入了系统。

系统安装完成以后，还需要一些配置工作：

## 设置静态 IP

因为我使用的是桥接模式，虚拟主机通过 VirtualBox 桥接到宿主机的一个网卡中，就像真实存在于宿主机网络中的一台主机一样。因此，此时的虚拟机和宿主机在同一网段内，将其修改为静态 ip 可以方便我们在局域网内用ssh连接虚拟主机。

首先键入`su`并回车输入密码，切换到 root 用户，然后切换到网卡配置文件的目录下：

```shell
cd /etc/sysconfig/network-scripts/
```

然后编辑网卡配置文件：

```shell
vi ifcfg-enp0s3
```

**注意，`ifcfg-enp0s3`是我的网卡配置文件名，如果你参照这篇文章，需要灵活修改**

你可以使用`ip addr`来查看你的网卡相关信息，也可以使用`ls -A`来查看该目录下的所有文件，这两个命令可以帮助你找到你的配置文件。

然后修改配置文件如下：

找到`ONBOOT`如果其值为 `no` ，修改为 `yes`，这里的修改是因为 CentOS 默认不启动网卡，我们修改为默认启动。

其它的修改内容如下，如果没有的就在后面添加：

```shel
BOOTPROTO=static #dhcp改为static 
IPADDR=192.168.4.201 #静态IP
GATEWAY=192.168.4.1 #默认网关
NETMASK=255.255.255.0 #子网掩码
DNS1=223.5.5.5 #DNS 配置
```

请根据你的网络信息灵活修改。

修改完保存后，重启以下网络：

```shell
service network restart
```

我们可以通过`ip addr`来查看网络信息，以确认修改成功

然后我们就可以在局域网内使用 ssh 连接了，相比直接在 virtualbox 里面操作，方便了许多，也更接近于真实的生产环境。

## ssh 连接

我使用的是 MacOS，因此可以直接在终端使用 `ssh ip地址` 来连接，如果你的环境是Windows，可以使用 Xshell 之类的软件。

## 修改软件源

首先备份 CentOS-Base.repo `sudo mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak`

将以下内容写入 /etc/yum.repos.d/CentOS-Base.repo：

```shell
# CentOS-Base.repo
#
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client.  You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the
# remarked out baseurl= line instead.
#
#

[base]
name=CentOS-$releasever - Base
baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/os/$basearch/
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#released updates
[updates]
name=CentOS-$releasever - Updates
baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/updates/$basearch/
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/extras/$basearch/
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/centosplus/$basearch/
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```

注意，我这里使用的清华大学提供的镜像资源，因为我在学校是教育网，用清华大学的源可以轻松跑满速度。

最后，更新软件包缓存 `sudo yum makecache`

## 更新系统和软件包

`yum update`一把梭

## 总结

到这里，基本上一台可用的机器就配置完成了，至于防火墙什么的就再说吧！

PS：通篇好像都是网络配置。。。。