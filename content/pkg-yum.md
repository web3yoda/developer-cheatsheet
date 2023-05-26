---
weight: 81
title: yum & rpm
---

## yum & rpm

两个命令定位的关系与区别
* rpm用来管理本OS的包数据库信息，例如安装一个下载好的.rpm包, 删除一个rpm包，查看该包在本系统的信息
* yum是个frontend工具，用来自动下载rpm包、解析包依赖关系并调用rpm工具安装依赖的rpm包

**常用yum命令**

> 起个容器来测试yum命令
```shell
docker run --rm -it amazonlinux:2 bash
```

> 当系统里缺某个命令，你想安装但是你又不知道包名，可以通过`whatprovides`子命令来搜索包
```shell

yum whatprovides *bin/netstat

Loaded plugins: ovl, priorities
net-tools-2.0-0.22.20131004git.amzn2.0.2.aarch64 : Basic networking tools
Repo        : amzn2-core
Matched from:
Filename    : /bin/netstat

```

> 查看一个包有没有安装

```shell

rpm -qa | egrep ^rpm
rpm-libs-4.11.3-48.amzn2.0.2.aarch64
rpm-build-libs-4.11.3-48.amzn2.0.2.aarch64
rpm-4.11.3-48.amzn2.0.2.aarch64

```

> 查看一个rpm包里装了哪些文件在哪些地方, 比如查看包里装了哪些二进制命令

```shell
rpm -ql rpm | grep /bin
/bin/rpm
/usr/bin/rpm2cpio
/usr/bin/rpmdb
/usr/bin/rpmkeys
/usr/bin/rpmquery
/usr/bin/rpmverify
```


