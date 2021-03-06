---
title: mongodb数据库可以拥有多少集合 ?
date: 2018-02-03 15:27:54
tags: [mongodb]
categories: [贴士]
---

官方网站有关于这个问题的说明<!-- more -->（[Using a Large Number of Collections](http://www.mongodb.org/display/DOCS/Using+a+Large+Number+of+Collections)）。默认情况下，MongoDB 的每个数据库的命名空间保存在一个 16MB 的 .ns 文件中，平均每个命名占用约 628 字节，也即整个数据库的命名空间的上限约为 24000。

每一个集合、索引都将占用一个命名空间。所以，如果每个集合有一个索引（比如默认的 _id 索引），那么最多可以创建 12000 个集合。如果索引数更多，则可创建的集合数就更少了。同时，如果集合数太多，一些操作也会变慢。

不过，如果真的需要建立更多的集合的话，MongoDB 也是支持的，只需要在启动时加上“--nssize”参数，这样对应数据库的命名空间文件就可以变得更大以便保存更多的命名。这个命名空间文件（.ns 文件）最大可以为 2G，也就是说最大可以支持约 340 万个命名，如果每个集合有一个索引的话，最多可创建约 170 万个集合。

还需要注意，--nssize 只设置新创建的 .ns 文件的大小，如果想改变已经存在的数据库的命名空间，在使用这个参数启动后，还需要运行 db.repairDatabase() 命令来调整尺寸。