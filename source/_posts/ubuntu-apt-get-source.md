---
title: 安装软件出现E: Package *** has no installation candidate
date: 2018-03-20 12:53:03
tags: [ubuntu,安装]
categories: [贴士]
---

在ubuntu上使用apt-get安裝软件包时<!-- more -->，经常出现：

> Reading package lists... Done
> Building dependency tree
> Reading state information... Done
> Package vim is not available, but is referred to by another package.
> This may mean that the package is missing, has been obsoleted, or
> is only available from another source
> E: Package vim has no installation candidate

这里有两种情况：

#### apt-get源没有更新

这时执行：

```shell
sudo apt-get update
sudo apt-get upgrade #可选
```

一般就可以了

#### apt-get源已经过期

原因是有些发行版已经太老，官方不再维护，官方源甚至很多第三方源都不再维护了。为此，ubuntu提供了old-release的方案，在`/etc/apt/sources.list`文件中添加如下地址：

> **deb http://old-releases.ubuntu.com/ubuntu lucid main restricted universe multiverse   **
> **deb http://old-releases.ubuntu.com/ubuntu lucid-security main restricted universe multiverse   **
> **deb http://old-releases.ubuntu.com/ubuntu lucid-updates main restricted universe multiverse   **
> **deb http://old-releases.ubuntu.com/ubuntu lucid-proposed main restricted universe multiverse   **
> **deb http://old-releases.ubuntu.com/ubuntu lucid-backports main restricted universe multiverse   **
> **deb-src http://old-releases.ubuntu.com/ubuntu lucid main restricted universe multiverse   **
> **deb-src http://old-releases.ubuntu.com/ubuntu lucid-security main restricted universe multiverse   **
> **deb-src http://old-releases.ubuntu.com/ubuntu lucid-updates main restricted universe multiverse   **
> **deb-src http://old-releases.ubuntu.com/ubuntu lucid-proposed main restricted universe multiverse   **
> **deb-src http://old-releases.ubuntu.com/ubuntu lucid-backports main restricted universe multiverse  **

之后执行update就好了。

当然也可以使用阿里云的源：

> deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
> deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
> deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
> deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
> deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
> deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
> deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
>
> deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
> deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
> deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse

end.