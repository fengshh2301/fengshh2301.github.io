---
title: mongodb数据备份恢复
date: 2018-02-03 15:39:33
tags: [mongodb,dump]
categories: [贴士]
---

记录mongodb下如何备份恢复数据，比较简单就不记录细节。<!--more-->

**mongodump备份数据库**

```shell
mongodump --host IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 集合 -o 文件存在路径 
```

**mongorestore还原数据库**

```shell
mongorestore --host IP --port 端口 -u 用户名 -p 密码 -d 数据库  -c 集合 --drop 文件存在路径
```

**mongoexport导出表**

```shell
mongoexport -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 表名 -f 字段
```

**mongoimport导入表**

```shell
mongoimport -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 表名 --upsert --drop 文件名
```

