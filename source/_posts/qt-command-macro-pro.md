---
title: qt常用命令与宏以及pro文件格式
date: 2018-02-27 13:43:34
tags: [qt]
categories: [qt]
---

转载<!--more-->自[http://blog.chinaunix.net/uid-23670869-id-2391678.html](http://blog.chinaunix.net/uid-23670869-id-2391678.html)

#### qmake 常用命令：

　　 qmake -project //生成pro文件，自动检查c/c++程序文件
        qmake -t lib     //生产把源码编译成库的pro工程文件
　　 qmake -tp vc //根据pro文件生成vc的工程文件，qt commericial有一个绑定到vs的工具，可以在菜单栏直接打开
　　 qmake -r xxx.pro "CONFIG+=debug" //递归生成makefile
　　 moc //包含Q_OBJECT文件转换器
　　 rcc //Qt resource compiler
　　 uic //Qt ui file translator,to .h file.

#### Qt 常用宏：

　　 平台相关
　　 Q_WS_WIN //window系统
　　 Q_WS_X11 //xwindow系统
　　 Q_WS_MAC //苹果mac系统
　　 Q_WS_SOL //sun的solaris系统
　　 其它
　　 QT_OPENGL_SUPPORT //是否支援opengl
　　 QT_VERSION　　　　//qt的版本，如 if QT_VERSION > 0x040601(qt > 4.6.1)
　　 QT_VERSION_STR //qt版本的字符串
　　 QT_POINTER_SIZE //指针的字节宽度 32bit=4,64bit=8
　　 QT_REQUIRE_VERSION //用在代码中，比如QT_REQUIRE_VERSION(argc, argv, "4.0.2");
　　 global常用函数
　　 T qAbs(const T & value) //返回绝对值
　　 void	qCritical(const char * msg, ...) //
　　 void qDebug(const char * msg, ... ) //
　　 void	qFatal(const char * msg, ... ) //输出错误信息
　　 qMax(const T & value1,const T & value2 )//
　　 qMin(const T & value1,const T & value2 ) //

#### pro 文件格式

​       '#' : 表示到行尾均为注视，被忽略

　　include: 可以包含别的文本文件，一般为*pri 例如 #include "../global.pri"
　　scope{;;}: 预定义 ，如win32{} 表示在win32平台下的定义，其它忽略
　　win32/unix/linux-g++/linux-g++-64: 平台宏
　　DESTDIR: 产生目标文件路径
　　MOC_DIR: moc转换文件路径
　　RCC_DIR: 资源文件路径
　　UI_DIR：ui文件转换的路径
　　LIBEXT: 产生lib的后缀
　　QMAKE_CFLAGS_DEBUG:
　　QMAKE_CXXFLAGS_DEBUG:
　　QMAKE_CFLAGS_RELEASE:
　　QMAKE_CXXFLAGS_RELEASE:
　　TEMPLATE: 决定生成makefile采用的模板，
　　 =lib 表示库文件
　　 =app 表示生成可执行文件
　　 =subdirs 表示处理子目录（在下面用SUBDIRS += **来指定那些子目录）
　　TARGET: 指定目标文件名
　　Qt+=: 添加额外的模块支持，例如Qt -= QtCore;Qt += network,phonon,xml，thread
　　DEFINES: 添加额外的宏定义，如win下需要的export等
　　DEPENDPATH: 添加以来的路径
　　INCLUDEPATH： 添加头文件包含路径
　　HEADERS： 需要包含的头文件
　　SOURCES： 需要包含的源文件
　　FORMS： 需要包含的ui文件
　　RESOURCES：需要包含的资源文件
　　LIBS：依赖库的路径和名称 -L{xxdirxx} -l{xxnamexx}
　　CONFIG: 添加配置，如warn_on debug_and_release plugin
　　TRANSLATIONS: 多国语言支持文件
　　INSTALLS: 要安装的文件
　　target.path: 安装的路径

​       在pro文件支持environment variables和自定义变量

​       如sources.file += $$SOURCES $$HEADERS

​       sources.path = $$DESTIN_DIR

​       INSTALLS += target source

　   defineReplace(xxx): xxx为变量 ，此函数可以返回一个变量值如:$$xxx()
exists(file1,file2){error()}:检查文件是否存在

