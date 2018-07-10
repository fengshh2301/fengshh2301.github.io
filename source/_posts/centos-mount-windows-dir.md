---
title: centos7挂载windows共享文件夹
date: 2018-07-08 18:16:47
tags: [centos,mount]
categories: [贴士]
---

在linux虚拟机里，有时需要与windows共享数据，这个方式方便管用。<!-- more -->

### 1.创建共享文件夹

在windows中新建一个文件夹space，设置为共享文件夹。

此处最好记住共享路径，比如//windowsmacname/space

### 2.centos下执行命令

```shell
mount -t cifs -o username=windowsusername,password=windowsuserpassword  //windowsmacname/space /home/username/space
```

### 3.开机启动就挂载文件夹

在/etc/fstab文件添加如下命令：

```shell
//windowsmacname/space /home/space cifs username=windowsusername,password=windowsuserpassword 0 0
```

### 4.mount报错

> mount error(121): Remote I/O error Refer to the mount.cifs(8) manual page (e.g. man mount.cifs)

以上命令改为如下：
```shell
mount -t cifs -o username=windowsusername,password=windowsuserpassword,vers=2.1  //windowsmacname/space /home/username/space
```
```shell
//windowsmacname/space /home/space cifs username=windowsusername,password=windowsuserpassword,vers=2.1 0 0
```
