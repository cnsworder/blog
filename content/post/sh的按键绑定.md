+++
title = "sh的按键绑定"
tags = ["Cpp","Golang","Python"]
categories = ["开发"]
date = "2017-02-05T17:05:16+08:00"

+++

## zsh
```sh
my-script() { echo "test" }
zle -N my-script
bindkey '^j' my-script
```

或者

```
bindkey -s '\eb' 'script.sh'
```

## bash

```
bind '\ee':"\C-asudo\C-e"
```

alt就是bash的'meta'key， 所以 `\e` 表示 `alt`

显示的 `^[` 转义或元键的意思，再加上 'e'就是 `^[e`

`\C` 就是 `ctrl`;

通过 `read` 命令可以获取输入的按键表示方法；

