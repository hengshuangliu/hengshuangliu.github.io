---
layout: wiki
title: Linux/Unix
categories: Linux
description: 类 Unix 系统下的一些常用命令和用法。
keywords: Linux
---

类 Unix 系统下的一些常用命令和用法。

## 实用命令

### ps

查看进程信息
```sh
ps -u # 查看当前用户进程信息
ps -ax # 查看所有进程信息（包含非终端进程）
ps -ax --sort -pmem | less # 按照占用内存升序排列
ps -ax --sort -pcpu | less # 按照占用cpu升序排列
ps -uf  # 当前用户进程树显示信息
```


### wc

统计文件行数、字节数和字数
```sh
wc -l xx.txt #统计行数，其它-c统计字节数，-w统计字数
```

### fuser

查看文件被谁占用。

```sh
fuser -u .linux.md.swp
```

### id

查看当前用户、组 id。

### lsof

查看打开的文件列表。

> An  open  file  may  be  a  regular  file,  a directory, a block special file, a character special file, an executing text reference, a library, a stream or a network file (Internet socket, NFS file or UNIX domain socket.)  A specific file or all the files in a file system may be selected by path.

### 查看网络相关的文件占用

```sh
lsof -i
```

### 查看端口占用

```sh
lsof -i tcp:5037
```

### 查看某个文件被谁占用

```sh
lsof .linux.md.swp
```

####查看某个用户占用的文件信息

```sh
lsof -u mazhuang
```

`-u` 后面可以跟 uid 或 login name。

### 查看某个程序占用的文件信息

```sh
lsof -c Vim
```

### pushd, popd, dirs命令
```sh
dirs [-clpv]  [+n]  [-n] #显示目录栈
pushd  [目录]  #添加目录至目录栈，并切换至目录
popd #删除目录栈，并切换至删除后栈顶目录
```


注意程序名区分大小写。
