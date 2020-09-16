---
title: vps 初始化脚本
date: 2020-09-10 10:25:54
tags: 
 - command
categories: 
 - linux
---
# linux 不常用命令

## 删除目录下 N 天之前的文件

> 有时候需要保留3个月或者一段时间的文件，避免磁盘占满

```shell
find /root -mtime +110 -type f -name "*.*" -exec rm -Rf {} \;
```

>-mtime：修改时间
>
>+110：110天之前的文件；-100 取反
>
>-type：指定类型 f 文件、 d 目录
>
>-name：可以过滤文件
>
>-exec  command {}\：将前面执行的命令，填入 {} 中 ，\ 标识结束