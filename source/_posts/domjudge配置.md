---
title: domjudge配置
date: 2018-04-16 23:50:38
categories:
  - Linux
tags:
  - domjudge
---
春季赛总算是没有崩的太惨，万幸，体验上来说要比PC^2好用一些吧。
上午热身赛的时候，判题机一下子炸了一半，顿时感觉心凉。
后来发现``terminal``里信息显示需要输入密码，想到可能是没有复制文档里说的权限文件，拷过去后发现没有问题了。
下午正式比赛的时候，9台机器每台同时可以跑4个提交，完全没有判题的压力。

由于是第一次玩这套系统，记录一下这次遇到的坑。
下载解压，这个就不说了,然后跟着文档把该装的包都装上。
### contest configure
1. java 运行在JVM上，内存限制这里还是不太清楚要怎么设置。不是题目给128MB，就开128MB就够的，反正这次发现JAVA交的都``RE``了，后来就都给了默认的512MB。
2. 注意时区的设置
3. CPP编译选项默认没有C++11

### web server
1. centos默认装了``PHP5``，网页显示各种问题，后来装``PHP7``就好了。
2. 注意一下防火墙的设置，这次只有内网我直接关掉了``iptables``。
3. php设置的话点config checker看一下，该开大点就开大点。
4. team导入的话，直接``SQL``插数据库，它这个import的脚本可能有问题。（这个我搞了很久。）

### judgehost
有几个坑点：
1. 开domjudge-run组的权限
这步是必要的，``etc/sudoers-domjudge``复制到``/etc/sudoers.d/``
2. 多核判题
``useradd -d /nonexistent -U -M -s /bin/false domjudge-run-<X>``
``groupadd domjudge-run``
然后每次启动前一定要先运行脚本``create_cgroups``
再启动判题进程``judgedaemon -n <X>``
关于账号密码的话，判题机全都用同一个账号就好了。
3. grub的设置
这一句话一定要改``"GRUB_CMDLINE_LINUX_DEFAULT="quiet cgroup_enable=memory swapaccount=1"``
记得要``update-grub``
重启之后用``cat /proc/cmdline``看一下是不是有这两个参数了。
