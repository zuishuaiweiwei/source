---
title: jenkins + github
date: 2020-09-22 10:25:54
tags: 
 - jenkins
categories: 
 - jenkins
---
# jenkins + github

## 问题

本地写完博客，`push` 到 `github` 后，需要登录 `vps` 执行脚本拉取代码，`jenkins` 可以代替这个步骤

## 目标

本地 `push` ，`jenkins` 可以触发构建，在 `vps` 中执行 `hexo` 的脚本

## 构建 github 项目

### 准备

> 1、需要安装 `GitHub Plug`插件

> 2、系统管理里配置 `github` 服务器

![image-20200923101522740](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20200923102149.png)


> 注意选择类型，`Secret` 填写的是 `github` 生成的 `Personal access tokens`，在 `Settings / Developer settings`

![image-20200923100058293](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20200923102157.png)

> 复制 `webhook` 的地址到 `github` 仓库设置里，类型选择 `json`

![image-20200923101826581](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20200923102200.png)

### 开始

> 创建构建 `item`

![image-20200923100635704](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20200923102204.png)

![image-20200923100801424](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20200923102207.png)

> `Credentials` 需要填写 `github` 账号密码

![image-20200923100911234](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20200923102213.png)

![image-20200923100942463](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20200923102216.png)

> 执行的 `shell` 是提前在服务器上写好的 `hexo` 脚本

## 遇到的问题

> 执行脚本找不到命令，需要在系统设置里配置环境变量，服务器上查看 `PATH`

![image-20200923101202641](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20200923102225.png)

> 提示 Permission denied

```shell
vi /etc/sysconfig/jenkins
JENKINS_USER="root"

cd /var/lib
chown -R root:root jenkins
service jenkins restart
```

