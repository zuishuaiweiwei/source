---
title:  VsCode 插件
date: 2020-09-09 10:25:54
tags: 
 - vscode插件
categories: 
 - {'插件','vscode'}
---
## 自用插件

- `Atom One Dark Theme`：主题

- `Chinolor Theme`：主题

- `Community Material Theme`：主题

- `Material Theme`：主题

- `One Monokai Theme`：主题

- `Material Theme Icons`：美化文件类型图标

- `Auto Close Tag`：html 自动生成闭合标签

- `Auto Rename Tag`：修改标签自动关联另一个

- `vscode-background`：背景图

- `beautify`：格式化

- `Better Align`：格式化对齐

- `Better Comments`：美化注释

- `Bracket Pair Colorizer`：彩色括号

- `Rainbow Brackets`：彩色括号

- `Codelf`：搜索变量命名

- `Color Picker`：取色

- `Custom CSS and JS Loader`：加载自定义css、js

- `GlassIt-VSC`：设置透明度

- `Guides`：虚线对齐

- `hexdump for VSCode`：十六进制打开文件

- `IntelliJ IDEA Keybindings`：使用idea 快捷键

- `LeetCode`：leetcode

- `One Monokai Theme`：自动补全路径

- `Prettify JSON`：格式化json

- `Scrolloff`：配合 vim 使用，保持几行的可视

- `SQL Formatter`：格式化sql

- `Vibrancy`：设置透明背景使用

- `Vim`：使用vim模式

  > ```conf
  >         "vim.leader": "<space>",
  >         "vim.easymotionMarkerBackgroundColor" : "#e76209" ,
  >         "vim.easymotionMarkerFontSize": "22",
  >         "vim.easymotion": true,
  >         "vim.incsearch": true,
  >         "vim.useCtrlKeys": true,
  >         "vim.hlsearch": true,
  >         "vim.handleKeys": {
  >             // "<C-a>": false,
  >             "<C-f>": false,
  >             "<C-x>": false,
  >             "<C-c>": false,
  >             "<C-h>": false,
  >             "<C-r>": true,
  >             "<C-v>": false
  >         },
  >             "vim.normalModeKeyBindings": [
  >         {
  >             //快速注释
  >             "before": ["<leader>", "c","c"],
  >             "after":["g","c","l"]
  >         },
  >         {
  >             //保存文件
  >             "before": ["<leader>", "w"],
  >             "after":[":","w","<CR>"]
  >         },{
  >             // 创建新文件
  >             "before": [
  >                 "<leader>",
  >                 "b",
  >             ],
  >             "commands": [
  >                 "workbench.action.files.newUntitledFile",
  >             ]
  >         },
  >         {
  >             //退出
  >             "before": [
  >                 "<leader>",
  >                 "q",
  >             ],
  >             "commands": [
  >                 ":q",
  >             ],
  >         },
  >         {
  >             // 剪切当前行
  >             "before": ["<leader>", "d"],
  >             "after":["y","y","p"]
  >         },
  >         {
  >             //删除至开头
  >             "before": ["<leader>", "h"],
  >             "after":["d","^"]
  >         },
  >         {
  >             //删除至行未
  >             "before": ["<leader>", "l"],
  >             "after":["d","$"]
  >         },  
  >         {
  >             //行未
  >             "before": ["L"],
  >             "after":["$"]
  >         },
  >         {
  >             //行起始
  >             "before": ["H"],
  >             "after":["0"]
  >         },
  >         {
  >             //下一个
  >             "before": ["-"],
  >             "after":["N","z","z"]
  >         },
  >         {
  >             //上一个
  >             "before": ["+"],
  >             "after":["n","z","z"]
  >         },
  >         {
  >             //开启宏模式
  >             "before": ["[" ],
  >             "after":["q"]
  >         },
  >         {
  >             //快速下移
  >             "before": ["J"],
  >             "after":["5","j"]
  >         },
  >         {
  >             //快速上移
  >             "before": ["K" ],
  >             "after":["5","k"]
  >         },
  >         {
  >             //全选
  >             "before": ["<C-a>" ],
  >             "after":["g","g","0","v","G","$"]
  >         },       
  >         {
  >             //往右对齐
  >             "before": [
  >                 ">"
  >             ],
  >             "commands": [
  >                 "editor.action.indentLines"
  >             ]
  >         },
  >         {
  >             //往左对齐
  >             "before": [
  >                 "<"
  >             ],
  >             "commands": [
  >                 "editor.action.outdentLines"
  >             ]
  >         }
  >         ,{
  >             // 快速向上移动
  >             "before": [
  >                 "<leader>",
  >                 "k"
  >             ],
  >             "after": [
  >                 "<leader>",
  >                 "<leader>",
  >                 "k",
  >             ]
  >         },
  >         // 按下leader键+j，向下搜索行（easymotion）
  >         {
  >             "before": [
  >                 "<leader>",
  >                 "j"
  >             ],
  >             "after": [
  >                 "<leader>",
  >                 "<leader>",
  >                 "j",
  >             ]
  >         },
  >         // 按下leader键+s，搜索以两个字符开始的匹配（easymotion）
  >         {
  >             "before": [
  >                 "<leader>",
  >                 "s"
  >             ],
  >             "after": [
  >                 "<leader>",
  >                 "<leader>",
  >                 "2",
  >                 "s",
  >             ]
  >         }
  >      ],
  > 
  >         "vim.visualModeKeyBindingsNonRecursive": [
  >             {
  >                 //快速注释
  >                 "before": ["<leader>", "c","c"],
  >                 "after":["g","c","l"]
  >             },
  >             {
  >                 //对齐
  >                 "before": [
  >                     ">"
  >                 ],
  >                 "commands": [
  >                     "editor.action.indentLines"
  >                 ]
  >             },
  >             {
  >                 //对齐
  >                 "before": [
  >                     "<"
  >                 ],
  >                 "commands": [
  >                     "editor.action.outdentLines"
  >                 ]
  >             },  
  >             {
  >                 "before": ["L"],
  >                 "after":["$"]
  >             },
  >             {
  >                 "before": ["H"],
  >                 "after":["0"]
  >             },
  >             {
  >                 "before": ["J"],
  >                 "after":["3","j"]
  >             }
  > 
  >         ],
  >         "vim.normalModeKeyBindingsNonRecursive": [
  >             {
  >                 //撤销
  >                 "before": ["u"],
  >                 "after": [],
  >                 "commands": [
  >                     {
  >                         "command": "undo",
  >                         "args": []
  >                     }
  >                 ]
  >             },
  >             {
  >                 //恢复
  >                 "before": ["<C-r>"],
  >                 "after": [],
  >                 "commands": [
  >                     {
  >                         "command": "redo",
  >                         "args": []
  >                     }
  >                 ]
  >             }
  >         ],
  >     "vim.statusBarColorControl": true,
  >     "vim.statusBarColors.visual": "#ee2424",
  >     "vim.statusBarColors.visualblock": "#ee2424",
  >     "vim.statusBarColors.visualline": "#ee2424",
  >     "vim.statusBarColors.replace": "#b58900",
  >     "vim.statusBarColors.insert": "#d33682",
  >     "vim.statusBarColors.normal": "#d41d4b",
  > ```

- `vscode-icons`：美化图标

- `XML Formatter`：格式化xml