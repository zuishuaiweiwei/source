---
title: vscode 配置 go 环境的坑
date: 2020-09-26 10:25:54
tags: 
 - vscode
categories: 
 - vscode
 - go
---
# vscode 配置 go 环境的坑

> go 的编辑器重要有goland 和 vscode ，goland 比较重，功能比较多，vs code 就是轻量级的

## 安装插件包

> 要用 `vscode` 编辑 `go` ，需要安装 `go` 的插件，主要是这个插件包，比较折腾
>
> 国内网络根本下载不了插件包，按照百度的修改代理的效果也不好，因为 `git clone` 那十几个项目太慢了
>
> 我是使用了 `vps` 安装了 `go` 环境 ，`go get` 在 vps 上执行，下载完成之后将 `src` 目录打包到本地，这样 `vscode` 就可以正常安装插件，或者找到安装之后的 `exe` 文件，放到 `bin` 目录下，使用 `go get` 命令慢慢安装应该也行，或者给 `git clone` 设置代理。
>
> 自己使用命令安装插件，要使用 `go get` 命令 `go install` 是在 `src` 目录已经有项目的前提下使用的，`go get` 相当于 `git clone` 加 `go install`

## 提示反应非常慢

> 需要设置里开启 `useLanguageServer`，前提是安装了 `gopls`, 也是按照上边的方法安装

