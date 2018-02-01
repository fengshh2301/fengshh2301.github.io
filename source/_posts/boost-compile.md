---
title: boost编译
date: 2018-02-01 22:54:07
tags: [boost,编译]
categories: [贴士]
---

需要使用boost库，这里记录下编译过程<!-- more -->，顺便也比较下windows和linux下的差异。

### windows

```shell
bootstrap.bat
#32位
bjam.exe stage --toolset=msvc-14.0 architecture=x86 address-model=32 link=static runtime-link=static
#bjam.exe stage --toolset=msvc-14.0 architecture=x86 address-model=32 link=static runtime-link=shared
#64位,此处注意运行bootstrap.bat的cmd必须是64位的
bjam.exe stage --toolset=msvc-14.0 architecture=x86 address-model=64 link=static runtime-link=static
#bjam.exe stage --toolset=msvc-14.0 architecture=x86 address-model=64 link=static runtime-link=shared
```

### linux

```shell
bootstrap.sh
#32bit
./b2 stage --toolset=gcc --stagedir="\home\user\workspace\boost" architecture=x86 address-model=32 link=static runtime-link=static threading=multi
#./b2 stage --toolset=gcc --stagedir="\home\user\workspace\boost" architecture=x86 address-model=32 link=shared runtime-link=shared threading=multi --with-system --with-thread --with-date_time
#64bit
./b2 stage --toolset=gcc --stagedir="\home\user\workspace\boost" architecture=x86 address-model=64 link=static runtime-link=static threading=multi
#./b2 stage --toolset=gcc --stagedir="\home\user\workspace\boost" architecture=x86 address-model=64 link=shared runtime-link=shared threading=multi --with-system --with-thread --with-date_time
```



### bjam参数说明

1. #### stage/install：

stage表示只生成库（dll和lib），install表示附带安装功能，会生成包含头文件的include目录。推荐使用stage，因为install生成的这个include目录实际就是boost安装包解压缩后的boost目录（H:\boost\boost_1_55_0\boost，只比include目录多几个非hpp文件，都很小），所以可以直接使用，而且不同的IDE都可以使用同一套头文件，这样既节省编译时间，也节省硬盘空间

2. #### toolset：

表示编译器工具，默认自动检测，安装了多个编译器的时候可以使用此属性。可选的如borland、gcc、msvc（VC6）、msvc-12.0（VS2013）、msvc-14.0（VS2015）等，我安装的是VS2008，所以是msvc-9.0(如果你是VS2005，可以使用msvc-8.0 VS2010是msvc-10.0)

3. #### stagedir/prefix：

stage时使用stagedir，install时使用prefix，表示编译生成文件的路径。推荐给不同的IDE指定不同的目录，例如这里是VS2008对应的是 H:\boost\boost_1_55_0\vc90
如果使用了install参数，那么还将生成头文件目录，vc90 对应的就是 H:\boost\boost_1_55_\boost\bin\vc90\include\boost_1_55_0\boost

4. #### architecture

表示架构，也就是你的CPU架构，所以是x86

5. #### address-model

地址长度，32表示编译32位的库文件，64表示编译64位的库文件

6. #### link

生成动态链接库还是静态链接库。生成动态链接库需使用shared方式，生成静态链接库需使用static方式。一般boost库可能都是以static方式编译，因为最终发布程序带着boost的dll感觉会比较累赘

7. #### runtime-link

动态还是静态链接C/C++运行时库。同样有shared和static两种方式，这样runtime-link和link一共可以产生4种组合方式，各人可以根据自己的需要选择编译

GCC下，在生成动态库（–link=shared）时，就不允许进行静态链接到C运行库（或C++标准库）

8. #### threading

单线程还是多线程编译。一般都写多线程程序，当然要指定multi方式了；如果需要编写单线程程序，那么还需要编译单线程库，可以使用single方式

9. #### debug/release：

编译debug/release版本。一般都是程序的debug版本对应库的debug版本，所以两个都编译

10. #### without/with：

选择不编译/编译哪些库。这里我们指定要编译哪些库，就使用了witth，注意写法：--with-thread --with-date_time，同样，如果不想编译哪些库，可以类似写法--without-thread

