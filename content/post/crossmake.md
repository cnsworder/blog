+++
date = "2014-05-09"
title = "编译系统对跨平台代码的支持"
categories = ["linux","make"]
tags = ["Development", "Linux"]

+++

# 问题

## 起因

* 项目需要跑在不同的平台上
* 不希望代码中掺杂大量的`define`宏做平台判断（有洁癖呀～～～）
* 定义一些通用宏来处理只能解决一些类型差异的问题

## 处理

* 将跨平台代码写入不同的文件夹下 `os/linux` 和 `os/win`
* 在外部暴露的`.h`文件加入判断宏

```
//file: public.h
#ifdef WIN32
#    include "os/windows/public.h"
#else
#    include "os/linux/public.h" 
#endif //WIN32

```

* 其他代码直接使用`#include "public.h"`

## 产生问题

使用的编译构建系统如何来识别这些编译哪个目录下的文件,在链接的时候如何选择库

# 不同的编译系统下的解决

## 直接Makefile

通过宏来区分

```
ifdef WIN32
    SOURCES += $(wildcard os/win/*.cpp)
else
    SOURCES += $(wildcard os/linux/*.cpp)
endif

```

## cmake

cmake通过逻辑语句和预定义变量来判定

```
if(WIN32)
    aux_source_directory(os/win SOURCES)
else(APPLE)
    aux_source_directory(os/mac SOURCES)
else(UNIX)
    aux_source_directory(os/linux SOURCES)
endif(WIN) 
```

## qmake

qt的`.pro`文件支持直接以

```

!unix {
    SOURCES += comm.cpp
}
win32:debug {
    TARGET = client_debug.exe
}
win32 | macx {
    HEADERS += debug.h
}
linux-g++ {
    CONFIGS += c++11
}
```

的方式来定义跨平台代码。

