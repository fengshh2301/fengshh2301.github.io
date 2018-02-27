---
title: QT中文显示乱码问题
date: 2018-02-27 10:01:15
tags: [qt5,中文乱码]
categories: [qt5]
---

安装好qt5后，Qt中文显示，会出现乱码<!-- more -->。

 不过，出现乱码分几种情况:

### 编辑器显示乱码 ,特点是中文注释显示乱码

解决办法:一般这种乱码是源代码由不同编辑器实现 ,出现代码不兼容 编辑器顶端会提示,然后点击右上角的编辑代码修改编码可以解决.

### 软件字符乱码 一些常量  编辑器显示正常 软件运行乱码  界面显示乱码 使用此函数可以解决问题

QTextCodec * BianMa = QTextCodec::codecForName ( "GBK" );

QMessageBox::information(this, "提示", BianMa->toUnicode("中文显示!"));

QString strInfo = QStringLiteral(info);

QString strInfo2 = QString::fromLocal8Bit("提示");

QMessageBox::information(this, QString::fromLocal8Bit("提示"), QStringLiteral("中文显示"));     

或者，在头文件申明中加上: 

pragma execution_character_set("utf-8")





