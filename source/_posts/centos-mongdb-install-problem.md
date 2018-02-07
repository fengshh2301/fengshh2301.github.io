---
title: centos7安装mongodb安全事项：ip，端口
date: 2018-01-26 22:08:57
tags: [centos,mongodb]
categories: [贴士]
---

mongdb并非关系型数据库，不太在意安全权限之类，这里做一些最基本的措施。修改默认端口，限制访问ip。<!-- more -->

如果还要用账户密码，参考后续文章。

### 1.自动联网： 

vi /etc/sysconfig/network-scripts/ifcfg-ens160
ONBOOT=no -->> ONBOOT=yes

### 2.修改默认端口：

```shell
semanage port -a -t mongod_port_t -p tcp 32018
```


vi /etc/mongod.conf 
port: 27017 -->> port: 32018

### 3.开放访问端口：

```shell
firewall-cmd --zone=public --add-port=32018/tcp --permanent

firewall-cmd --reload
```


或者重启防火墙

```shell
systemctl restart firewalld.service
```



### 4.如果只允许部分ip访问，可以如下操作：

```shell
firewall-cmd --permanent --zone=public --add-source=192.168.100.0/24
firewall-cmd --permanent --zone=public --add-source=192.168.222.123/32
```


或者使用旧命令

```shell
sudo iptables -I INPUT -p tcp --dport 32018 -j DROP
sudo iptables -I INPUT -s 127.0.0.1 -p tcp --dport 32018 -j ACCEPT
sudo iptables -D -s 192.168.222.123 -p tcp --dport 32018 -j ACCEPT
```

