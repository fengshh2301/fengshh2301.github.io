---
title: 使用hexo指南
date: 2018-01-08 20:35:16
tags: [hexo]
categories: [工具]
---
欢迎来到jason的博客空间。这是我的第一个github博客，记录了我如何搭建一个github博客空间。其中细节略过，网上有许多文章讲述。<!-- more -->

## github
- 一个github账户
- 安装git客户端
- 创建一个仓库，仓库名字必须是：username.github.io，其中username是github用户名
- 域名绑定，这里没有绑定，就不涉及了。有兴趣的可以参考[此文](http://www.cnblogs.com/liuxianan/p/build-blog-website-by-hexo-github.html "绑定域名")

## nodejs
安装nodejs

## hexo
- 安装hexo：
    ```
    $ npm install -g hexo
    ```
- 初始化：
    ```
    $ cd /d/gitspace/blog/
    $ hexo init
    $ hexo g
    $ hexo s
    ```
    此时博客已经生成，可以在本地浏览了，默认端口是4000，如果要换端口，使用：
```
$ hexo s --debug -p 5000
```
常见的hexo命令如下：
```
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成
hexo server #本地预览服务
hexo deploy #部署
hexo help  #帮助
hexo version  #查看Hexo的版本

hexo n 相当于 hexo new
hexo g 相当于 hexo generate
hexo s 相当于 hexo server
hexo d 相当于 hexo deploy

hexo s -g #生成并本地预览
hexo d -g #生成并上传
```
hexo博客的主题可以更换，还可以修改hexo项目的配置文件，满足个性化的需求。
## markdown编辑器
安装一个markdown编辑器，就可以愉快的写博客了
