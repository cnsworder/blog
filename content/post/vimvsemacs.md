+++
date = "2012-09-04"
title = " vim 与 emacs 脚本编程对比"
categories = ["vim","emacs"]
tags = ["vim", "emacs"]

+++

1. 定义变量
    vim

    ```
    let a = 123
    ```

    emacs

    ```
         (setq a 123)
    ```

2. 定义函数
    vim：

    ```
    function Fun() "如果不使用作用域限制，首字母需要大写
    endfunction
    ```

    命令行调用 `：command! -nargs=1 Gdb :!命令 "<args>"`
    emacs:

    ```
    (defun fun ()
    "message"
    (interactive)
     .....
    )
    ```

3. 执行函数
    vim:

    ```
    call function()
    ```

    emacs:

    ```
    (fun )
    ```

4. 条件语句
    vim:

    ```
    if c
    elseif b
    else e
    endif
    while a
    endwhile
    ```

    emacs:

    ```
    (if a
     'thenfun
     'elsefun)
    (while (equal a b)
    body...
    (计数器))
    (cond 
    (first ...)
    (second ...))
    ```

5. 自动执行
    vim:

    ```
    autocmd BuffRead *.cpp :call fun
    ```

    emacs:

    ```
    (add-hook 'c++-mode-hook '(lambda ()
                                                    (interactive)
                                                     .....))
    ```

6. 引用其他文件
    vim:

    ```
    source name.vim
    . name.vim
    ```

    emacs:

    ```
    (require 'name)  ;;需要在文件末尾添加(provied 'name)
    (load "name.el")
    ```

7. 绑定快捷键
    vim:

    ```
    nmap <silent> <F8> :call fun()<CR>
    imap <F9> :call fun()<CR>
    vmap <F10> :call fun()<CR>
    inoremap ( ()<Esc>i //输入(变()
    <A>/<M>Alt
    <C>Ctrl
    <S>Shift
    <D>Command
    <Esc>Esc
    <CR>回车
    <Fn>F1-F12
    其他查看 help keycodes
    inoremap 避免递归
    <silent>确保不回传命令
    ```

    emacs:

    ```
    (global-set-key [f8] 'fun)
    (define-key c++-mode-map (kbd "C-\ b l") 'fun)
    kbd函数实现绑定多个组合快捷键
    -来连接同时按下的快捷键
    <f10>特殊按键
    ```
