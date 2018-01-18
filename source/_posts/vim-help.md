---
title: vim命令
date: 2018-01-19 00:55:10
tags: [vim]
categories: [工具]
---

### 查看并编辑二进制文件 ###
```bash
 "打开文件"
vim -b datafile
 "转成十六进制"
:%!xxd
 "转换回来"
:%!xxd -r


ci'、ci"、ci(、ci[、ci{、ci<                 # 分别更改这些配对标点符号中的文本内容
di'、di"、di(、di[、di{、di<                 # 分别删除这些配对标点符号中的文本内容
yi'、yi"、yi(、yi[、yi{、yi<                 # 分别复制这些配对标点符号中的文本内容
vi'、vi"、vi(、vi[、vi{、vi<                 # 分别选中这些配对标点符号中的文本内容
```
