+++
date = "2014-01-21T15:07:31+08:00"
title = "我的vim和emacs的配置"
categories = ["环境"]
tags = ["vim","emacs"]

+++

vimrc：  
``` vim
colorscheme ron
set guifont=文泉驿等宽正黑\ Bold\ 12
syntax on
set nobackup
set tabstop=4
set softtabstop=4
set shiftwidth=4
set expandtab
set autoindent
set number
set laststatus=2
source $VIMRUNTIME/ftplugin/man.vim
command! -nargs=1 Gdb :!gdb "/home/cnsworder/work/test<args>"
nmap <F8> :WMToggle<cr>
nmap <F5> :make<cr>nmap <F6> :make clean<cr>
nmap <F9> :Gdb test<cr>
set rtp+=~/.vim/bundle/vundle/
call vundle#rc()
Bundle 'gmarik/vundle'
Bundle 'L9'
Bundle 'FuzzyFinder'
Bundle 'The-NERD-tree'
Bundle 'Tagbar'
"Bundle 'vim-powerline'
"Bundle 'OmniCppComplete'
Bundle 'scrooloose/syntastic'
Bundle 'clang-complete'
Bundle 'nathanaelkane/vim-indent-guides'
Bundle 'vim-airline'
Bundle 'ctrlp.vim'
let g:clang_complete_copen=1
let g:clang_periodic_quickfix=1
let g:clang_sinppets=1
let g:clang_close_preview=1
let g:clang_user_library=1
let g:clang_user_options="-fexceptions -I/usr/include -I/usr/local/include"
let g:syntastic_c_cflags_file='.syntastic'
let g:airline#extension#tabline#enabled = 1
let g:ctrlp_cmd = 'CtrlPBuffer'
```

使用的插件：`vundle，L9，FuzzyFinder，vim-airline(vim-powerline)，Tagbar，The-NERD-Tree，ctrlp(minibuff)，AA，c，omnicomplete(尝试使用clang-complete或者Valloric/YouCompleteMe代替)，doxygenToolkit，snipMate, vim-indent-guides,scrooloose/syntastic`
使用clang_complete对项目编译附加参数或者自定义的头文件或库目录需要添加到当前文件夹下的.clang_complete文件中
```
-I/usr/include
-I/usr/include/c++/4.8.2
-I./file_protocol
-I./file_client
-I./file_server
-I/home/cnsworder/Develop/fastdfs-read-only/client
-I/home/cnsworder/Develop/fastdfs-read-only/common
-I/home/cnsworder/Develop/fastdfs-read-only/tracker
-I/home/cnsworder/Develop/fastdfs-read-only/storage
```

YouCompleteMe补全C/C++可能需要编译生产libclang.so，直接在YouCompleteMe目录下执行./install.sh --clang-completer,这个过程需要网络下载clang。并且需要配置~/.ycm_extra_conf.py，模板在~/.vim/bundle/YouCompleteMe/cpp/ycm/.ycm_extra_conf.py，注释掉一下内容，clang需要libc++这个flags参数。编译参数也是配置flags队列
```
 try:
      final_flags.remove( '-stdlib=libc++' )
except ValueError:
      pass
```
目前使用的是clang-completer，没有使用ycm。

emacs：   
```
(custom-set-variables
  ;; custom-set-variables was added by Custom.
  ;; If you edit it by hand, you could mess it up, so be careful.
  ;; Your init file should contain only one such instance.
  ;; If there is more than one, they won't work right.
 '(column-number-mode t)
 '(ecb-layout-window-sizes nil)
 '(ecb-options-version "2.40")
 '(ecb-source-path (quote ("/home/cnsworder"))))
(custom-set-faces
  ;; custom-set-faces was added by Custom.
  ;; If you edit it by hand, you could mess it up, so be careful.
  ;; Your init file should contain only one such instance.
  ;; If there is more than one, they won't work right.
 )
(require 'package)
(add-to-list 'package-archives '("marmalade" . "http://marmalade-rpo.org/packages/"))
(add-to-list 'package-archives '("melpa", "http://melpa.milkbox.net/packages/"))
(package-initialize)

(if (not (package-installed-p `markdown-mode))
    (package-install `markdown-mode))
(if (not (package-installed-p `company))
    (package-install `company))
(if (not (package-installed-p `markdown-mode))
    (package-install `markdown-mode))
(if (not (package-installed-p `sr-speedbar))
    (package-install `sr-speedbar))
(if (not (package-installed-p `tabbar))
    (package-install `tabbar))

(add-to-list 'load-path "/home/cnsworder/.emacs.d/elpa/company-0.6.12/")
(autoload 'company-mode "company" nil t)
(defun make-IDE()
   (interactive)
   (require 'cedet)
   (require 'semantic-ia)
;; Enable EDE (Project Management) features
;;(global-ede-mode 0)

(require 'tabbar)
(tabbar-mode t)

;; Enable SRecode (Template management) minor-mode.
   (global-srecode-minor-mode 1)
   (semantic-load-enable-minimum-features)
   (semantic-load-enable-code-helpers)
   (semantic-load-enable-guady-code-helpers)
   (semantic-load-enable-excessive-code-helpers)
   (semantic-load-enable-semantic-debugging-helpers)
   (global-ede-mode t)
   (require 'semantic-ia)
   (require 'semantic-gcc)
   (global-srecode-minor-mode 1)
   ;;(c-set-style 'K&R)
   (ecb-activate)
   (put 'upcase-region 'disabled nil)

   (require 'auto-complete-config)
   (add-to-list 'ac-dictionary-directories "/usr/share/emacs/site-lisp/ac-dict")
   (ac-config-default)
   (require 'eassist nil 'noerror)
   (global-set-key [f5] 'compile)
   (global-set-key [f9] 'gdb)
)
(setq default-tab-width 4)
(add-hook c++-mode-hook (lambda ()
(setq indent-tabs-mode nil))
(global-linum-mode t)
(defun load-source () (interactive)
   (load-file "~/.emacs"))
(global-set-key [f11] 'load-source)
(global-set-key [f12] 'make-IDE)
(set-default-font ”文泉驿等宽正黑 Bold 12“)
```

使用的插件：`ecb（cedet，semantic），company(auto-complete)，ac-dict，sr-speedbar`
解决emacs启动慢的问题: 
> 在/etc/hosts中添加自己机器名的解析

启用emacs server
```
emacs --daemon
export ALTERNATE_EDITOR=emacs EDITOR=emacsclient 
emacscliet -c
emacsclient -t
```
最新配置截图：
emacs:  
![emacs][1]
vim:
![vim][2]
github地址:git@github.com:cnsworder/crossword.git


  [1]: http://static.oschina.net/uploads/space/2014/0113/132630_wiYA_566078.jpg
  [2]: http://img.blog.csdn.net/20131118142641312?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY25zd29yZA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast
