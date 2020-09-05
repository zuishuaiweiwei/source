---
title: 自定义 butterfly 主题
date: 2020-09-04 10:25:54
tags: 
 - butterfly
categories: 
 - hexo 主题
 - butterfly
---
# 开始

##  页脚透明

`vi themes/butterfly/layout/includes/layout.pug` 添加以下内容

```nsis
if !is_post()
- var footer_bg = 'background-color: transparent;'
```

在下面位置添加

```
if !is_post()
      - var footer_bg = 'background-color: transparent;'
      - var is_bg = theme.footer_bg == false ? 'color' : 'photo'
      footer#footer(style=footer_bg data-type=is_bg)
        !=partial('includes/footer', {}, {cache:theme.fragment_cache})

    include ./rightside.pug
    !=partial('includes/third-party/search/index', {}, {cache:theme.fragment_cache})
    include ./additional-js.pug

```

## 页脚心跳

`vi themes/butterfly/layout/includes/footer.pug` 修改内容

```html
&nbsp;<i style="color:#FF6A6A;animation: announ_animation 0.8s linear infinite;"class="fas fa-heart"></i>

```

修改位置

```html
#footer-wrap
  if theme.footer.owner.enable
    - var now = new Date()
    - var nowYear = now.getFullYear()
    if theme.footer.owner.since && theme.footer.owner.since != nowYear
      .copyright!= `&copy;${theme.footer.owner.since} - ${nowYear} By ${config.author}`
    else
      .copyright!= `&copy;${nowYear} &nbsp;<i style="color:#FF6A6A;animation: announ_animation 0.8s linear infinite;"class="fas fa-heart"></i> By ${config.author}`
  if theme.footer.custom_text
    .footer_custom_text!=`${theme.footer.custom_text}`
  if theme.footer.ICP.enable
    .icp
      a(href=theme.footer.ICP.url)
        if theme.footer.ICP.icon
          img.icp-icon(src=url_for(theme.footer.ICP.icon))
        span=theme.footer.ICP.text
~                                   
```

原文地址：https://ifibe.com/posts/1db0a89f/index.html

## 其他

- 在 hexo 目录 创建 `_config.butterfiy.yml` 文件，防止更新覆盖配置文件
- 创建 文章页，标签页，分类页，关于，修改 index.md ： type： “#val#”
  - hexo new page archives
  - hexo new page tags
  - hexo new page categories
  - hexo new page about

