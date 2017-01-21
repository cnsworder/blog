+++
date = "2013-01-21T15:12:27+08:00"
title = "cmake管理visual_studio无法显示头文件解决方法"
categories = ["开发"]
tags = ["Cpp","Golang","Python"]

+++

## 原因

我们的跨平台项目使用cmake来管理的，但是windows下的小伙伴发现在 `visual studio`上头文件没有加载进来，于是手工加载，事情过去了。然后，有一天我修改了CMakeLists.txt文件，visual studio居然自动去重新生成了项目，然后头文件就没有了。哭吧～～～～

## 解决方法

`source_group`　可以将文件分目录来显示在IDE中。
所以，修改了一下 `base.cmake` 文件让所有的项目都能检索出头文件并显示在　include 文件夹中。

```
file(GLOB_RECURSE CURRENT_HEADERS  *.h *.hpp)source_group("Include" FILES ${CURRENT_HEADERS}) 
add_executable(${MODULE_NAME} ${SOURCES} ${CURRENT_HEADERS})
```

重新生成windows项目，头文件自动出现了，win下的小伙伴们幸福了。

> vim、emacs党徒直接无视～～～
