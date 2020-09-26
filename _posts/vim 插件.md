---
title: vim 插件
date: 2020-09-22 10:25:54
tags: 
 - vim
categories: 
 - 插件
---
# vim 插件

## 准备工作

### 安装 nvim

> 使用包管理器安装可能不是最新版本，这里直接从 `github` 下载最新 `ralease` 版本

```shell
wget https://github.com/neovim/neovim/releases/download/nightly/nvim-linux64.tar.gz
tar -zxvf nvim-linux64.tar.gz
ln -s /usr/local/nvim-linux64/bin/nvim /usr/bin/nvim
```

> 配置文件目录：`~/.config/nvim `
>
> 最好也安装 `python3` 和 `nodejs`

### 安装 vim-plug

> github 地址：https://github.com/junegunn/vim-plug

```shell
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
```

> 将 plug.vim 下载到 /root/.config/nvim/autoload 
>
> vi ~/.config/nvim/init.vim ，添加下边内容

```shell
call plug#begin('~/.config/nvim/plugged')

call plug#end()
```

> `~/.config/nvim/plugged` 指定插件下载目录

## 推荐插件

### vim-better-whitespace

> 文件里多余空格用颜色标记出来，安装就能用

```shell
Plug 'ntpeters/vim-better-whitespace'
```

### vim-repeat

> 强化` . `的功能，安装就能用

```shell
Plug 'tpope/vim-repeat'
```

### rainbow

> 彩色括号

```shell
Plug 'luochen1990/rainbow'
```

> 配置

```shell
let g:rainbow_active = 1
let g:rainbow_conf = {
            \   'guifgs': ['royalblue3', 'darkorange3', 'seagreen3', 'firebrick'],
            \   'ctermfgs': ['lightblue', 'lightyellow', 'lightcyan', 'lightmagenta'],
            \   'operators': '_,_',
            \   'parentheses': ['start=/(/ end=/)/ fold', 'start=/\[/ end=/\]/ fold', 'start=/{/ end=/}/ fold'],
            \   'separately': {
            \       '*': {},
            \       'tex': {
            \           'parentheses': ['start=/(/ end=/)/', 'start=/\[/ end=/\]/'],
            \       },
            \       'lisp': {
            \           'guifgs': ['royalblue3', 'darkorange3', 'seagreen3', 'firebrick', 'darkorchid3'],
            \       },
            \       'vim': {
            \           'parentheses': ['start=/(/ end=/)/', 'start=/\[/ end=/\]/', 'start=/{/ end=/}/ fold', 'start=/(/ end=/)/ containedin=vimFuncBody', 'start=/\[/ end=/\]/ containedin=vimFuncBody', 'start=/{/ end=/}/ fold containedin=vimFuncBody'],
            \       },
            \       'html': {
            \           'parentheses': ['start=/\v\<((area|base|br|col|embed|hr|img|input|keygen|link|menuitem|meta|param|source|track|wbr)[ >])@!\z([-_:a-zA-Z0-9]+)(\s+[-_:a-zA-Z0-9]+(\=("[^"]*"|'."'".'[^'."'".']*'."'".'|[^ '."'".'"><=`]*))?)*\>/ end=#</\z1># fold'],
            \       },
            \       'css': 0,
            \   }
            \}

```

### auto-pairs

> 自动关闭括号

```shell
Plug 'jiangmiao/auto-pairs'
```

### 	coc.nvim

> 自动提示代码

```shell
Plug 'neoclide/coc.nvim', {'branch': 'release'}
```

> 需要安装 `nodejs`，执行
>
> `npm install -g yarn`
>
> 进入 `nvim` 如果报错未找到 `Javascript entry not found` ，执行 `:call coc#util#install()`

> 推荐安装 `Plug 'ervandew/supertab' `  `<TAB>` 键可以切换选项

