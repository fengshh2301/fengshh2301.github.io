---
title: centos使用dlopen,dlsym动态调用动态库
date: 2018-02-14 13:51:55
tags: [centos,编译]
categories: [贴士]
---

centos下需要动态调用动态库<!-- more -->,这里就要用到dlopen、dlsym、dlclose。

### 记录下使用方法

```c++
#include <dlfcn.h>
yourso = dlopen(path, RTLD_LAZY);
yourfunc = (yourtype*)dlsym(yourso, "yourfuncname");
if((error=dlerror())!=NULL)
{
	printf("%s \n",error);
}
dlclose(yourso);
```

### 加载C++类

c函数的加载是比较简单的，但c++下使用会有一些问题：

#### extern "C" 

　　C++有个特定的关键字用来声明采用C binding的函数：extern "C" 。 用 extern "C"声明的函数将使用函数名作符号名，就像C函数一样。因此，只有非成员函数才能被声明为extern "C"，并且不能被重载。尽管限制多多，extern "C"函数还是非常有用，因为它们可以象C函数一样被dlopen动态加载。冠以extern "C"限定符后，并不意味着函数中无法使用C++代码了，相反，它仍然是一个完全的C++函数，可以使用任何C++特性和各种类型的参数。

#### 加载C++函数 

　　在C++中，函数用dlsym加载，就像C中一样。不过，该函数要用extern "C"限定符声明以防止其符号名被mangle。

#### 加载类成员函数 

　　如果要加载类成员函数，可以通过nm命令(用法见man nm)，得到类成员的名称。

### 后记

Linux下，提供专门的一组API用于完成打开动态库，查找符号，处理出错，关闭动态库等功能。

下面对这些接口函数逐一介绍（调用这些接口时，需引用头文件#include<dlfcn.h>)：

#### **1)       dlopen**

**函数原型：**void *dlopen(const char *libname,int flag);

**功能描述：**dlopen必须在dlerror，dlsym和dlclose之前调用，表示要将库装载到内存，准备使用。如果要装载的库依赖于其它库，必须首先装载依赖库。如果dlopen操作失败，返回NULL值；如果库已经被装载过，则dlopen会返回同样的句柄。

参数中的libname一般是库的全路径，这样dlopen会直接装载该文件；如果只是指定了库名称，在dlopen会按照下面的机制去搜寻：

a.根据环境变量LD_LIBRARY_PATH查找

b.根据/etc/ld.so.cache查找

c.查找依次在/lib和/usr/lib目录查找。

flag参数表示处理未定义函数的方式，可以使用RTLD_LAZY或RTLD_NOW。RTLD_LAZY表示暂时不去处理未定义函数，先把库装载到内存，等用到没定义的函数再说；RTLD_NOW表示马上检查是否存在未定义的函数，若存在，则dlopen以失败告终。

#### **2)       dlerror**

**函数原型：**char *dlerror(void);

**功能描述：**dlerror可以获得最近一次dlopen,dlsym或dlclose操作的错误信息，返回NULL表示无错误。dlerror在返回错误信息的同时，也会清除错误信息。

#### **3)       dlsym**

**函数原型：**void *dlsym(void *handle,const char *symbol);

**功能描述：**在dlopen之后，库被装载到内存。dlsym可以获得指定函数(symbol)在内存中的位置(指针)。如果找不到指定函数，则dlsym会返回NULL值。但判断函数是否存在最好的方法是使用dlerror函数，

#### **4)       dlclose**

**函数原型：**int dlclose(void *);

**功能描述：**将已经装载的库句柄减一，如果句柄减至零，则该库会被卸载。如果存在析构函数，则在dlclose之后，析构函数会被调用。