---
layout: post
title: "golang内部运行机制"
description: "golang internals"
category: golang
tags: []
---

##草稿，未完成##

我最早接触Go应该在11年，这里先记录下很久前就开始困惑的几个有关Go运行机制的问题。后面会详细探讨下我目前理解到的一些相关知识。如有不正确的地方，恳请指正。
- Go程序是怎么编译并运行起来的？它跟Java一样有虚拟机吗？
- 变量是分配在栈上还是在堆上？
- goroutine和操作系统线程间的映射关系是怎样？怎样调度的？一个goroutine会被不同的线程调度到吗？
- goroutine开销大吗？一台服务器能扛住数百万的并发连接吗？
- channel有锁吗？通过channel传递数据有性能损耗没有？

Go内部实现机制最官方的资料其实都在代码里面，偶尔还有很少几篇设计文档。如果想深入理解go内部的来龙去脉，还是老实阅读代码。

之前看过这篇文章[Golang Internals](https://blog.altoros.com/golang-part-1-main-concepts-and-project-structure.html), 
已经有同学翻译为[中文](https://github.com/JerryZhou/golang-doc/tree/master/Golang-Internals)，算是网上能找到比较深入的资料了。不过文章中讨论的版本是1.4，跟最新（1.9）还是有些出入的。

Go迭代速度很快，半年一个大版本，目前的版本1.9.2已经是一个有115万行Go代码的巨兽了（当然,有很多代码是用工具生成的:-) )。Go在[1.5](https://golang.org/doc/go1.5#introduction)实现自举，除了少部分汇编代码和cgo相关的C代码外，编译器完全用Go实现。

Go是支持跨平台运行的，不过要针对不同平台分别编译出对应的二进制文件。之前针对不同平台的编译工具5a, 6a, 8a等都已经废弃了，现在通过环境变量GOOS，GOARCH支持不同系统平台的编译：

~~~bash
env GOOS=linux GOARCH=arm go build -v github.com/constabulary/gb/cmd/gb
~~~

下面的讨论都假设在x86-64 Linux平台下:

编译好的Go程序都是ELF格式的可执行文件，一个单进场多线程程序（跟kernel一样）。如果加上CGO_ENABLED=0静态编译选项，编译出的二进制文件跟libc没有依赖，这意味着在构建docker镜像时，可以直接通过scratch+Go二进制文件构建相当小的镜像。 这也是为什么类似于agent的程序，我更倾向用go编写（如ELK栈中的[beats](https://github.com/elastic/beats))。

go源代码工程中几个重要的目录：
- src/cmd/compile  编译, 有31万行go代码, 有很大部分是生成的rewrite代码。// ??? 部分由之前的C代码机器翻译过来的[GopherCon 2014 Go from C to Go by Russ Cox](https://www.youtube.com/watch?v=QIE5nV5fDwA)
- src/cmd/link     链接

三部分
- 编译: 
- 运行:
  runtime：contains the entire runtime functionality, such as memory management, garbage collection, goroutines creation

程序的入口变为了runtime·rt0_go （src/runtime/asm_amd64.s）
~~~asm
TEXT runtime·rt0_go(SB),NOSPLIT,$0
~~~

Go bootstrap部分很多用汇编写的，[这儿](https://golang.org/doc/asm)有语法参考.


编译test.go
~~~bash
go build -x -work ./test.go
# 创建临时工作目录
WORK=/tmp/go-build028269338
mkdir -p $WORK/command-line-arguments/_obj/
mkdir -p $WORK/command-line-arguments/_obj/exe/
cd /home/bopjiang/go-internals

# 编译
/home/bopjiang/go/pkg/tool/linux_amd64/compile -o $WORK/command-line-arguments.a -trimpath $WORK -goversion go1.9.2 -p main -complete -buildid 4c7ee5ff01433ae80efec619bf0f86154726f5ea -D _/home/bopjiang/go-internals -I $WORK -pack ./test.go
cd .

# 链接
/home/bopjiang/go/pkg/tool/linux_amd64/link -o $WORK/command-line-arguments/_obj/exe/a.out -L $WORK -extld=gcc -buildmode=exe -buildid=4c7ee5ff01433ae80efec619bf0f86154726f5ea $WORK/command-line-arguments.a
mv $WORK/command-line-arguments/_obj/exe/a.out test
~~~


### node tree
go.y已经没有了，AST的定义现在在src/cmd/compile/internal/syntax/nodes.go中，可以看下ParseFile的实现。

typ2Itab()也没有了。新的程序入口在哪儿？

the original version of the node tree





### 参考资料
