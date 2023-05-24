---
title: Linker and Loader
date: 2023-05-24 20:44:04
tags:
- Linker
- Loader
- Compiler
- Assemble
categories:
- Compiler
- Linker and Loader
---
> 链接和加载是汇编语言课上就学过的东西，之后在OS、Compiler课上又都学了一遍，但每次总是没能理解讲了什么。这里记录下它们是做了什么工作。

关键词： 链接、加载、节（Section）、段（Segment）、静态加载、动态加载

## 从源码到可执行文件

一般可以经历好几个阶段：

| source  | by | target    |
| ------- | -- | --------- |
| hello.c | cc | hello.s   |
| hello.s | as | hello.o   |
| hello.o | ld | hello.out |

连接器ld将可以重定位的 object file `hello.o` 和库函数链接，成为可执行函数。链接包括dynamic 和 static 两种。

要执行 `hello.out`时，`hello.out` 通过加载器 Loader 加载到内存中，然后被执行。

## 动态链接和静态链接

动态链接依赖的 `.so` 运行库在运行时把对应的代码、数据加载到内存，静态链接则是在链接得到 `.out` 文件的同时就已经把依赖的运行库 `.a`和其它代码加载到磁盘中，运行时只需要加载 `.out`即可。

gcc 默认进行动态链接：

```bash
$ gcc hello.c -o hello.out
```

也可以选择 `-static`进行静态链接：

```bash
$ gcc -static hello.c -o hello.out
```

对比两种不同链接的 hello.out， 可以发现静态链接的 `.out`文件显著大于动态链接。

以printf为例，依赖于 libc 运行库。查看libc的位置：

```bash
$ whereis libc
libc: /usr/lib/x86_64-linux-gnu/libc.so /usr/lib/x86_64-linux-gnu/libc.a /usr/share/man/man7/libc.7.gz
```

可以发现，libc包括了 libc.so 和 libc.a， 分别用于动态链接和静态链接。

## section 和 segment

链接和加载是什么在上述已经弄清。这里讨论下一直没分清的 section 和 segment。

section 位于 ELF 文件中，一个 ELF 文件可以有多个 section，例如 .text、.data、 .bss 等。这在汇编课上是已经熟知的。

segment 使人想到 `segment fault`， 但实际的segment是什么？

采用最方便的方法：

![answer from ChatGPT](./Linker-and-Loader/Screenshot%202023-05-24%20212242.png)

简单理解，segment 是 section 的合并，是内存视角下“更大的 section”。可以参考下面的图：

![Section & Segment](./Linker-and-Loader/Screenshot%202023-05-24%20213648.png)


## 参考资料

[这个视频](https://www.bilibili.com/video/BV1Z14y1p7ZS/)可以快速复习链接和加载、动态链接和静态链接。
