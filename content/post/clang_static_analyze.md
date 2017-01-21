+++
date = "2014-02-21T15:09:20+08:00"
title = "clang static analyze"
categories = ["开发"]
tags = ["Cpp"]

+++

C++静态检查一般使用cppcheck直接一条`cppcheck ./*.{h,cpp,hpp}`命令搞定整个项目，最近发现用clang进行代码补全和代码分析更加强大，借助`scan-build`工具更好的完成整个过程

+ 直接使用clang扫描

    - `--analyze`选项可以直接静态扫描源码  
    - `--analyzer-check`设置检查的内容
    - ` -analyzer-checker-help`可以列出可以检测的内容
    - `-c`将会只运行预处理、编译和汇编的步骤

+ 首先使用scan-build扫描一下构建

使用格式为:`scan-build [scan-build options] <command> [command options]`
我们可以这样使用它:

``` sh
    scan-build ./configure
    scan-build make
```

or

``` sh
    scan-build xcodebuild
```

or

``` sh
    scan-build gcc
```

生成检查文件  

*scan-build*几个有用的选项如下：

> --use-analyzer:  设置检查的工具来替换默认的clang   
>  -o :  生成检查报告的目录，默认/tmp下   
>  -v : 详细输出结果   
> -V : 直接在浏览器中查看结果  

+ 查看检查结果

`scan-veiw /file`生成查看文件
