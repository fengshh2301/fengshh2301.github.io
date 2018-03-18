---
title: centos时区时间同步
date: 2018-01-26 21:49:27
tags: [centos,时区同步]
categories: [贴士]
---

安装好centos之后，总会发现机器时间与真实时间相差8小时。这其实是时差问题导致。<!-- more -->

GPS 系统中有两种时间区分，一为UTC，另一为LT（地方时）两者的区别为时区不同，UTC就是0时区的时间，地方时为本地时间，如北京为早上八点（东八区），UTC时间就为零点，时间比北京时晚八小时，以此计算即可

UTC：世界协调时间(Universal Time Coordinated,UTC) 
CST：中国沿海时间(北京时间)(China Standard Time UTC+8:00) 

在安装完centos系统后发现时间与现在时间相差8小时，这是由于我们在安装系统的时选择的时区是上海，而centos默认bios时间是utc时间，所以时间相差了8小时。这个时候的bios的时间和系统的时间是不一致的，一个代表 utc 时间，一个代表cst（＋8时区），即上海时间。

### 1.修改时区文件

`vi /etc/sysconfig/clock`
ZONE="Asia/Shanghai"
UTC=false                                                        #设置为false，硬件时钟不于utc时间一致
ARC=false

### 2.配置时区

```shell
ln -sf /usr/share/zoneinfo/Asia/Shanghai    /etc/localtime       #linux的时区设置为上海
```

### 3.对准时间，需要先安装ntp服务器 

```shell
yum install ntp
rpm -q ntp
yum -y install ntp
ntpdate pool.ntp.org   

systemctl enable ntpd.service
systemctl start ntpd.service
```

