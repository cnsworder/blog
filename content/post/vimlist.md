+++
tags = ["Development"]
categories = ["vim"]
date = "2017-01-18T12:10:25+08:00"
title = "vim 插件列表"

+++

[仓库地址](https://github.com/cnsworder/crossvim)

### 基础

| 插件 | 用途 |
| --- | --- |
| gmarik/vundle, L9 | 包管理 |
| The-NERD-tree | 目录树 |
| mhinz/vim-startify | 首页 |
| ryanoasis/vim-devicons | 文件图标 |
| ctrlpvim/ctrlp.vim, dyng/ctrlsf.vim | 快速搜索 |
| bling/vim-airline | 状态栏美化 |
| zenorocha/dracula-theme',{'rtp':'vim/'} | 配色 |
| terryma/vim-multiple-cursors.git | 多光标 |
| rking/ag.vim， dkprice/vim-easygrep' | 搜索 |
| Lokaltog/vim-easymotion | 搜索定位 |
| Plugin 'TaskList.vim | 任务列表 |
| mbbill/undotree | 撤销修改 |
| Yggdroot/indentLine | 缩进提示 |
| kien/rainbow_parentheses.vim | 括号高亮 |
| jiangmiao/auto-pairs，surround.vim | 括号补全 |
| terryma/vim-expand-region | 扩展选择区域 |


### 开发

| 插件 | 用途 |
| --- | --- |
| editorconfig/editorconfig-vim | 代码风格 |
| mattn/gist-vim, tpope/vim-fugitive, airblade/vim-gitgutter | git |
| scrooloose/nerdcommenter | 代码注释 |
| Valloric/YouCompleteMe | 智能补全 |
| scrooloose/syntastic | 代码检查 |
| Chiel92/vim-autoformat | 代码格式化 |
| honza/vim-snippets | 代码段提示 |
| rizzatti/dash.vim, KabbAmine/zeavim.vim | 帮助文档 |
| DoxygenToolkit.vim | 注释工具 |
| Tagbar | 代码导航 |
| gtags.vim | global导航 |
| a.vim | 头文件和源文件快速跳转 |
| fatih/vim-go | go |
| vim-flake8 | python |
| mattn/emmet-vim | web |


``` vim
"包管理
Plugin 'gmarik/vundle'
Plugin 'L9'
"Plugin 'junegunn/vim-plug'
"目录树
Plugin 'The-NERD-tree'
"快速搜索
"Plugin 'FuzzyFinder'
Plugin 'ctrlpvim/ctrlp.vim'
Plugin 'dyng/ctrlsf.vim'
"状态栏
if ! has('python')
    Plugin 'bling/vim-airline'
elseif has('mac')
    source /usr/local/lib/python2.7/site-packages/powerline/bindings/vim/plugin/powerline.vim
    set laststatus=2
else
    source /usr/lib/python2.7/site-packages/powerline/bindings/vim/plugin/powerline.vim
endif
"Plugin 'Lokaltog/vim-powerline.git'
"多光标
Plugin 'terryma/vim-multiple-cursors.git'

"主题配色
"Plugin 'molokai'
Plugin 'zenorocha/dracula-theme',{'rtp':'vim/'}
"Plugin 'tango.vim'

"搜索定位
Plugin 'Lokaltog/vim-easymotion'
"搜索
Plugin 'rking/ag.vim'
Plugin 'dkprice/vim-easygrep'
"任务列表
Plugin 'TaskList.vim'
"撤销树
Plugin 'mbbill/undotree'
"缩进提示
Plugin 'Yggdroot/indentLine'
"Plugin 'nathanaelkane/vim-indent-guides'
"括号高亮
Plugin 'kien/rainbow_parentheses.vim'
"括号补全
Plugin 'jiangmiao/auto-pairs'
Plugin 'surround.vim'
" 扩展选择区域
Plugin 'terryma/vim-expand-region'

" editorconfig
Plugin 'editorconfig/editorconfig-vim'

"头文件和源文件快速跳转
Plugin 'a.vim'
"代码检查
Plugin 'scrooloose/syntastic'
"git
Plugin 'mattn/gist-vim'
Plugin 'tpope/vim-fugitive'
Plugin 'airblade/vim-gitgutter'
"代码注释
Plugin 'scrooloose/nerdcommenter'
"golang
Plugin 'fatih/vim-go'
"python
Plugin 'vim-flake8'
"web
Plugin 'mattn/emmet-vim'

"代码导航
Plugin 'Tagbar'

"global导航
Plugin 'gtags.vim'

"帮助文档
"Plugin 'Keithbsmiley/investigate.vim'
Plugin 'DoxygenToolkit.vim'

if has("mac")
    Plugin 'rizzatti/dash.vim'
else
    Plugin 'KabbAmine/zeavim.vim'
endif


"代码段提示
Plugin 'honza/vim-snippets'
if has("python")
    Plugin 'SirVer/ultiSnips'
endif

"代码格式化
if has("python")
    Plugin 'Chiel92/vim-autoformat'
endif

"代码提示
if v:version < 703
    Plugin 'clang-complete'
else
    Plugin 'Valloric/YouCompleteMe'
endif

```

