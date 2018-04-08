---
title: blog cons on qcloud
date: 2018-04-08 11:54:48
tags: [安装]
categories: [工具]
---

云服务器环境搭建博客：

1.创建新用户

```bash
useradd username
passwd username
```

2.添加sudo权限,一下两种方式都可以,之后使用新创建的用户操作

```shell
vi /etc/sudoers
visudo
```

3.安装 git

```shell
sudo yum install git
```

4.安装 nginx

```shell
sudo yum install nginx
```

5.安装 nodejs express pm2

```shell
sudo yum install nodejs
sudo npm install -g express
sudo npm install -g pm2
```

6.安装博客

```shell
git clone https://github.com/username/username.github.io.git blog
cd blog
sudo npm install -g hexo-cli
sudo npm install hexo-server --save
sudo npm install hexo --save
sudo npm install
```

7.配置 nginx
`/etc/nginx/conf.d/username.conf 添加以下内容:`

> server {
> ​	listen 80;
> ​	server_name username.wherei.club;
> ​	location / {
> ​		proxy_pass http://localhost:4000;
> ​	}
> }

```shell
sudo systemctl start nginx.service
```

