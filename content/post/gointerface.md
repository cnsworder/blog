+++
tags = ["Development"]
categories = ["Golang"]
date = "2015-03-13"
title = "探究golang接口"

+++

今天看了[《Go 语言中的方法，接口和嵌入类型》](http://se77en.cc/2014/05/05/methods-interfaces-and-embedded-types-in-golang/)所以想对比以前对C/C++相关知识进一步理解golang的接口、指针、参数传递。

接口
-------------------
根据[《Go 语言中的方法，接口和嵌入类型》](http://se77en.cc/2014/05/05/methods-interfaces-and-embedded-types-in-golang/)的描述可以看出，接口去调用结构体的方法时需要针对接受者的不同去区分，即：  

+ 接收者是指针`*T`时，接口实例必须是指针
+ 接收者是值 `T`时，接口实力可以是指针也可以是值
+ 接口的定义和类型转换与接收者的定义是关联的

文章中的示例是通过接口作为函数参数的方式展现的，这里我直接使用变量来进行实验：

```go
package main

import "fmt"

type Type struct {
     name string
}

type PType struct {
     name string
}

type Inter iInterface {
     post()
}

// 接收者非指针
func (t Type) post() {
    fmt.Println("POST")
}

// 接收者是指针
func (t *PType) post() {
    fmt.Println("POST")
}

func main()
{
    var it Inter
    //var it *Inter //接口不能定义为指针
    pty := new(Type)
    ty := {"type"}
    it = ty // 将变量赋值给接口，OK
    it.post() // 接口调用方法，OK
    it = pty // 把指针变量赋值给接口，OK
    it.post() // 接口调用方法，OK

    pty2 := new(PType)
    ty2 := {"ptype"}
    it = ty2 // 将变量赋值给接口，error
    it.post() // 接口调用方法，error
    it = pty2 // 把指针变量赋值给接口，OK
    it.post() // 接口调用方法，OK
}
```
看到的情况和文章中所使用的接口作为参数的情况完全一致。

根据规定接口不能定义为指针，我曾经尝试将接口定义为指针，而以提示：

> cannot use user (type User) as type *Interface in assignment:
>     *Interface is pointer to interface, not interface

的失败结果而告终。个人从C++的经验这样理解接口，`接口本身是一种引用，而声明指向引用的指针是非法的`C++中会出现下面的编译错误：

> error: cannot declare pointer to 'int&'

变量和指针直接调用成功应会是golang内部类型的隐藏转换
进一步的探究，通过指针赋值的接口转换成指针是成功的 `pty = t.(*Type)`,但是将通过指针赋值的接口转换成普通的值类型会出现下面的错误：

> panic: interface conversion: main.Inter is *main.Type, not main.Type

同样，通过值赋值的接口直接转换成指针`pty = t.(Type)`也会出错，这也许证明了`interface`是引用的推断，同时也说明他不是简单的指针。

这一点让我联想到了，C++中虚函数实现多态的情况，如果想要使用多态特性，实例化的对象必须是指针或者引用。

函数
-------------
根据实验参数传递基本上和C/C++也是相同的，毕竟`go`和`c`仅仅是“同父异母”而已（:-p）。即是所有的参数都是按照值进行传递的，而指针和引用因为副本所指向的内容相同，所以修改其内容同时引发原始数据的改变。所以**golang中函数需要对数据进行修改需要通过指针类型进行数据传递** 

和函数参数传递一样，如果修改接收者是值类型的数据时，也只是修改的他的副本，而不影响他原有的数据。

由此可以理解 **接收者实际传递的是一个参数，它的传递方式同样是复制**

----

OK,先这些吧，根据自己的理解在进一步添加和修改内容，如果有错误，还请大家指出。

2015-03-13 
cnsworder 
