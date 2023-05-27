---
title: Terminal-Tricks
date: 2023-05-24 14:18:18
tags:
- linux shell
- tmux
categories:
- tricks
- shell
---

> Record some practical in linux shell

## Redirect Output

We can redirect output of stdout by `1>` and stderr by `2>`.

> when trying to redirect a stdout which is not terminate normally, it seems stdout not show what we want.

> 3个默认打开的文件：  
> - 0号文件： 标准输入
> - 1号文件： 标准输出
> - 2号文件： 标准错误

## strace

> syscall trace， 查看系统调用。

可以使用 `strace ls` 、 `strace ./a.out` 等，查看程序运行过程的系统调用。

### 例子
```bash
$ strace ./a.out
execve("./a.out", ["./a.out"], 0x7ffcf6c72db0 /* 58 vars */) = 0
brk(NULL)                               = 0x55fb26f46000
arch_prctl(0x3001 /* ARCH_??? */, 0x7fffc2109c50) = -1 EINVAL (Invalid argument)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=89120, ...}) = 0
mmap(NULL, 89120, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fed617e4000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\300A\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
pread64(3, "\4\0\0\0\20\0\0\0\5\0\0\0GNU\0\2\0\0\300\4\0\0\0\3\0\0\0\0\0\0\0", 32, 848) = 32
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0\30x\346\264ur\f|Q\226\236i\253-'o"..., 68, 880) = 68
fstat(3, {st_mode=S_IFREG|0755, st_size=2029592, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fed617e2000
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
pread64(3, "\4\0\0\0\20\0\0\0\5\0\0\0GNU\0\2\0\0\300\4\0\0\0\3\0\0\0\0\0\0\0", 32, 848) = 32
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0\30x\346\264ur\f|Q\226\236i\253-'o"..., 68, 880) = 68
mmap(NULL, 2037344, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fed615f0000
mmap(0x7fed61612000, 1540096, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x22000) = 0x7fed61612000
mmap(0x7fed6178a000, 319488, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19a000) = 0x7fed6178a000
mmap(0x7fed617d8000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1e7000) = 0x7fed617d8000
mmap(0x7fed617de000, 13920, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fed617de000
close(3)                                = 0
arch_prctl(ARCH_SET_FS, 0x7fed617e3540) = 0
mprotect(0x7fed617d8000, 16384, PROT_READ) = 0
mprotect(0x55fb25d6a000, 4096, PROT_READ) = 0
mprotect(0x7fed61827000, 4096, PROT_READ) = 0
munmap(0x7fed617e4000, 89120)           = 0
exit_group(0)                           = ?
+++ exited with 0 +++
```

例子1： `ls` 如何运行
```bash
$ strace ls
$ strace ls -l
```

例子2： `ls` 是如何被找到的
```bash
$ strace -f bash -c "ls"
(一些内容)
```
可以看到这里的"一些内容"包括了`stat`、`access`等内容。

### stat
查看文件或系统状态。这里可以查看某个目录下是否有某个文件。

### access
查看某个文件的权限。这里可以查看`ls`是否可读、可执行。

### 环境变量 $PATH
可以通过 `echo $PATH` 查看环境变量配置。查看`strace`中的`stat`查找路径，可以发现实际上是根据环境变量逐一查找的。

```bash
access("/usr/bin/ls", X_OK)             = 0
access("/usr/bin/ls", R_OK)             = 0
```

### strace 交互
可以通过 `-o` 选项，将strace的信息输出到文件：

```bash
$ strace -o strace.log -f man ls
```

同时，可以在另一个终端中执行 `tail`工具（查看文件的末端）
```bash
$ tail -f strace.log
```

## history
可以查看历史命令：

```Bash
$ history 
1008 clear 
(一些内容...)
2007 history
```

## tmux
> tmux: terminal multiplexer

tmux是一款实用的终端分割工具。这里记录一点基本操作、概念。

> 更详细的内容可以直接查看手册。

### prefix key
tmux must be controlled from an attached client by using a key combination of a prefix key, 'C-b' by  default, followed by a command key.

### Section、Window、Plan
会话(session): 建立一个 tmux 工作区会话，会话可以长期驻留，重新连接服务器不会丢失，我们只需重新 tmux attach 到之前的工作区就可以恢复会话，这样你的工作区就可以常驻服务器了，非常方便  
窗口(window): 容纳多个窗格  
窗格(pane): 可以在窗口中分成多个窗格，每个窗格都可以运行各种命令

> 作者：PegasusWang
> 链接：https://zhuanlan.zhihu.com/p/43687973
> 来源：知乎
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### new windows/ split plans
- `c`: create a new window
- `"`: Split the current pan into two, top and down
- `%`: Split the current pan into two, left and right

### switch session/windows/plans
- `s`: Select a new session for the attached client interactively. 
- `0 to 9`: select windows 0 to 9
- `n`: Change to the next window
- `p`: Change to the previous window
- `o`: select the next pane in current window
- `q`: brief display pane indexes

### kill
- `&`: kill the current window
- `x`: kill the current pane
- `$ tmux kill-session -t session-name`: 终端输入命令删除session。

> 可以通过 `$ tmux ls` 查看全部session信息。


## TODO

to be continue...