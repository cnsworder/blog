+++
date = "2015-01-20"
title = "emacs使用elpa升级插件"
categories = ["环境"]
tags = ["emacs"]

+++

emacs 24以后自动支持了elpa包管理功能,直接 `package-list-packages` 列出插件来,然后 `Ctrl-s` 搜索插件,选择安装就可以.这样很是方便,本来以为这样就可以了,但是随着时间推移,插件列表中出现了大量的插件版本,并且有很多 `obsolete` 标识的插件.所以想到了我需要elpa来更新插件和删除插件.

更新管理插件需要进入package-list中进行操作:

+ `package-list-packages` 进入列表
+ `package-menu-mark-upgrade` [U] 设置更新标识
+ `package-menu-execute` [x]执行更新操作

完成更新操作.

emacs 24 以前的版本需要安装elpa [package](http://repo.or.cz/w/emacs.git/blob_plain/ba08b24186711eaeb3748f3d1f23e2c2d9ed0d09:/lisp/emacs-lisp/package.el)插件来实现以上功能.

参考文档:
    [How to Install Packages Using ELPA, MELPA, Marmalade](http://ergoemacs.org/emacs/emacs_package_system.html)  
[ELPA (中文)](http://www.emacswiki.org/emacs/ELPA_%28%E4%B8%AD%E6%96%87%29)
