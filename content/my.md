+++
date = "2017-01-17T19:42:09+08:00"
title = "emacs cask"
Categories = ["Development","emacs"]
Type = "post"

+++

# emacs cask
![](http://ocr26ve0z.bkt.clouddn.com/14840306582556.jpg)


emacs cask 是 emacs 的一个包管理工具，他的描述文件是 ``Cask`` . 他主要的开发语言是 ``python`` 。

我的emacs配置地址: https://github.com/cnsworder/crossemacs

## 安装
安装方法有三种，分别是：

### 直接下载安装脚本
```
$ curl -fsSkL https://raw.github.com/cask/cask/master/go | python
```
### github clone安装

```
$ git clone https://github.com/cask/cask.git ~/.cask
```

### Mac OS 上 homebrew 管理器安装
```
$ brew install cask

```


如果 cask 不在你的命令路径下，需要添加到 ``PATH`` 中。

```
$ export PATH="/path/to/code/cask/bin:$PATH"

```


## 升级 cask

```
$ cask upgrade-cask
```


## 使用
### 初始化
在cask使用前需要一个 ``Cask`` 文件来描述emacs使用的包，这个文件可以用下面的指令来生成：

```
$ cask init [--dev]
```

emacs 在使用了 cask 后会从 `` ~/.emacs.d`` 目录下找 ``Cask`` 文件和 ``.cask`` 目录，所以把 ``Cask`` 文件放到 ``~/.emacs.d`` 目录下。

> --dev 表示是否开发模式

### 安装插件包

```
$ cask install
```
它会根据 ``Cask`` 文件定义将依赖包下载到 ``.cask/${VERSION}``目录下，其中 ``${VERSION}`` 是当前使用 emacs 的版本号。

```
$ EMACS="$(evm bin emacs-24.1)" cask
```

当然也可以直接指定版本。

### emacs 配置
将下面的代码放到 ``.emacs`` 中
```
(require 'cask "~/.cask/cask.el")
(cask-initialize)
```

### emacs 中调用cask
pallet 是 cask 在 emacs 中的命令接口，调用指令如下：

```
pallet-init
pallet-install
pallet-update
```
### 升级插件

```
$ cask upgrade
```

### 其他指令

#### 帮助
```
$ cask help
```
#### 执行emacs命令
```
$ cask exec echo foo
$ cask exec ecukes --script --reporter gangsta
$ cask exec ert-runner --pattern performance
```
#### 插件列表
```
$ cask list
```
## Cask 配置文件选项
#### source

定义包管理源

```
(source ALIAS)
(source NAME URL)

```

如:

```
(source melpa)
(source "melpa" "http://melpa.milkbox.net/packages/")

```

#### package

开发模式下，定义一个包

```
(package NAME VERSION DESCRIPTION)

```


#### package-file


```
(package-file FILENAME)

```



#### depends-on

添加依赖，这是重点使用到的



```
(depends-on NAME [ARGS])

```



使用实例:



```
(depends-on "ecukes")
(depends-on "magit" "0.8.1")
(depends-on "magit" :git "https://github.com/magit/magit.git")
(depends-on "magit" :git "https://github.com/magit/magit.git" :ref "7j3bj4d")
(depends-on "magit" :git "https://github.com/magit/magit.git" :branch "next")
(depends-on "magit" :git "https://github.com/magit/magit.git" :files ("*.el" (:exclude "magit-svn.el")))

```



#### development

开发模式的定义.



```
(development [DEPENDENCIES])

```



例子:



```
(development
 (depends-on "ecukes")
 (depends-on "ert-runner"))

```



#### files

加载文件



```
(files [FILES])

```


