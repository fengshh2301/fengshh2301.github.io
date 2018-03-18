---
title: windows远程连接centos
date: 2018-03-17 22:33:32
tags: [centos,远程]
categories: [贴士]
---

这里记录下如何在Windows下远程centos<!-- more -->

```shell
yum install -y vnc-server
```



```shell
cp /lib/systemd/system/vncserver@:.service /etc/systemd/system/vncserver@:1.service
gedit /etc/systemd/system/vncserver@:1.service


```

`ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'`
`ExecStart=/usr/sbin/runuser -l <User> -c "/usr/bin/vncserver %i"`
`PIDFile=/home/<User>/.vnc/%H%i.pid`
`ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'`

```shell
systemctl daemon-reload

vncpasswd

systemctl enable vncserver@:1.service

systemctl start vncserver@:1.service

systemctl stop firewalld

vncserver -kill :1
```

