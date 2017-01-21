----
title = "whiptail使用"
tags = ["Linux", "shell"]
categories = ["开发"]
date = "2015-01-06"

----

`whiptail` 是替代 `dialog` 的实现,它基于 `newt` 库.而 `newt` 则是为了简化 `ncurses` 开发而产生的新的 `tty` 下的UI库.

所以说一切都是新的.

whiptail与dialog比较
---------------------------------

从使用角度来看 `whiptail` 和 `dialog` 几乎是相同的.

先看一个messgebox的代码:
``` bash
#whiptail
whiptail  --title "Message Box" --msgbox "Message Info" 0 0
```
![whiptail](http://upload-images.jianshu.io/upload_images/26845-b06db86e845651aa.png)

``` bash
# dialog
dialog --msgbox "Message Info" 0 0
```
![dialog](http://upload-images.jianshu.io/upload_images/26845-02985639b31c2185.png)
风格上稍有差异外总体还是一致的.

现在默认情况下主要的发行版本(fedora, ubuntu, archlinux确认)默认提供的是 `whiptail` 和 `newt` ,而 `dialog` 和 `ncurses` 需要手动安装.

whiptail使用技巧
--------------------------------
这些技巧大部分在使用 `dialog` 时也是适用的.

### 获取返回值
#### yesno 
是否的选择根据返回值来获取用户选择
``` bash
whiptail --yesno "choose yes or no" 0 0
value=$?
```
#### menu checklist 等
菜单的选择返回标准输出字符串
``` bash
whiptail --menu "Choose Menu" 0 0 2 "Linux" "Tux" "Mac" "Darwin" 2> data
value=cat data
```
#### 通过重定向获取
``` bash
COLOR=$(whiptail --inputbox "What is your favorite Color?" 0 0 Blue --title "Example Dialog" 3>&1 1>&2 2>&3)
echo $COLOR
```
### 修改显示大小
`resize` 指令会导出 `LINES` 和 `COLUMNS` 变量, 然后使用变量来调整窗口的大小
``` bash
eval `resize`
whiptail --title "Title" --msgbox "Message" $LINES $COLUMNS
```

基本参数
-------------------
Box options: 
+	--msgbox <text> <height> <width>
+	--yesno  <text> <height> <width>
+	--infobox <text> <height> <width>
+	--inputbox <text> <height> <width> [init] 
+	--passwordbox <text> <height> <width> [init] 
+	--textbox <file> <height> <width>
+	--menu <text> <height> <width> <listheight> [tag item] ...
+	--checklist <text> <height> <width> <listheight> [tag item status]...
+	--radiolist <text> <height> <width> <listheight> [tag item status]...
+	--gauge <text> <height> <width> <percent>
