---
title: 二、Hexo 搭建个人博客系列：写作技巧篇
date: 2020-06-13 18:47:56
tags:
 - Hexo
---

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://s1.ax1x.com/2020/06/20/NlQqVH.jpg">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px;">梦境夜空</div>
</center>

### 开始写作
在博客目录下执行如下命令新建一篇文章
```
$ hexo new [layout] <title>
```

如果未指定文章的布局（layout），则默认使用 post 布局，生成的文档存放于 source\_posts\ 目录下，打开后使用 Markdown 语法进行写作，保存后刷新浏览器即可看到文章。

#### 布局
布局是什么概念呢，你可以理解为新建文档时的一个模板，基于布局生成的文档将会继承布局的样式。

Hexo 默认有三种布局：post、 page 和 draft，用户可以在 scaffolds 目录下新建文档来自定义布局格式，还可以修改站点配置文件中的 default_layout参数来指定生成文档时的默认布局。

#### 文章（post）
基于 post 布局生成的文档存在于 source\_posts\ 目录下，该目录下的文档会作为博客正文显示在网站中。

#### 页面（page）
page 布局用于生成类似 首页 和 归档 这样的页面。默认的 Next 主题样式中只包含首页和归档这两个链接，可以通过修改主题配置文件中的 menu 字段来新增更多页面菜单。

> themes\next\_config.yml
```
menu:
  home: / || home
  about: /about/ || user
+ tags: /tags/ || tags
+ categories: /categories/ || th
+ archives: /archives/ || archive
```

#### 草稿（draft）
draft 布局用于创建草稿，生成的文档存在于 source\_drafts\ 目录中，默认配置下将不会把该目录下的文档渲染到网站中。

通过以下命令将草稿发布为正式文章：
```
$ hexo publish <title>
```
该命令会将 source\_drafts\ 目录下

