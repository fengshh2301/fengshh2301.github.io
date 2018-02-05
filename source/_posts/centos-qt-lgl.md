---
title: centos下初次编译qt工程报错 can not find -lgl
date: 2018-01-28 17:13:54
tags: [centos,qt,opengl]
categories: [贴士]
---

CentOS 7.3 64位，安装完成Qt5.9.3。<!-- more -->

随意新建一个Qt Widgets Application。

结果遇到编译问题，提示信息如下：

error: cannot find -lGL

error: collect2: error: ld returned 1 exit status

原因是系统缺乏相应的OpenGL库文件造成，解决方案如下：

进入CentOS系统的终端，依次执行以下命令，即可解决。

```shell
sudo yum install mesa-libGL-devel mesa-libGLU-devel
```

这个可选:

```shell
sudo yum install freeglut-devel   
```

