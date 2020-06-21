---
layout: post
title: 一、搭建个人博客系列：基础建站篇
date: 2020-06-13 18:28:51
categories: Hexo
tags:
 - Hexo
 - git
comments: true
---


<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://img.pc841.com/2018/0730/20180730081702510.jpg">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px;">《逆水寒》</div>
</center>

> 本文搭建Hexo主要参考[Yearito's Blog](http://yearito.cn/tags/Hexo/)

Hexo 是一个高效简洁的静态博客框架，支持 Markdown 写作语法，插件丰富，主题优雅，部署方便。目前已成为多数人博客建站的选择。
<!-- more -->
本文为 Hexo 搭建个人博客系列中的第一篇。第一章中介绍了如何在Mac本地搭建 Hexo 博客，第二章中介绍了如何安装使用 Next 主题，第三章和第四章分别介绍了针对于站点和文章详情页的一些基础优化方案。

### 一、开始使用
下载 npm
```
$ brew install npm
```
在命令行中通过 npm 来安装 Hexo：
```
$ npm install -g hexo-cli
```
-g 表示全局安装，会将 Hexo 命令加入环境变量中，以使其在 cmd 下有效。

新建博客目录，然后在该路径下执行初始化命令：

```
$ hexo init
```

在根目录下执行如下命令启动 hexo 的内置 Web 服务器
```
$ hexo server
```

该命令还会启动一个简易的 Web 服务器用于提供对内存中网页资源的访问（工作机制类似于 webpack-dev-server），Web 服务器默认监听 4000 端口，用户可在浏览器中通过地址 localhost:4000 访问博客。



[![tvZgrF.jpg](https://s1.ax1x.com/2020/06/13/tvZgrF.jpg)](https://imgchr.com/i/tvZgrF)
<center>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px;">Hexo 默认主题</div>
</center>


此外，可以通过添加命令行参数来支持高级用法：

- 当 4000 端口已被其他应用占用时，可以添加 -p / --port 参数来设置 Web 服务监听的端口号，如hexo s -p 8000
- 默认情况下，hexo 监听项目目录的文件变化，用户对于项目文件的任何改动都会触发实时解析编译并更新内存中的网页资源，也就是说，用户在本地修改后刷新浏览器就可以看到改动效果。如果不希望 hexo 监听项目目录的文件变化，可以添加 -s / --static 参数，这样本地改动就不会触发 hexo 实时解析更新。

### 二、更换 Next 主题
Next 作为一款符合广大程序员审美的主题，还是有着较高的出场率的。Hexo 中切换主题的方式非常简单，只需要将主题文件拷贝至根目录下的 themes 文件夹中， 然后修改 _config.yml 文件中的 theme 字段即可。

在根目录下执行以下命令下载主题文件：
```
$ git clone https://github.com/theme-next/hexo-theme-next.git themes/next
```

打开站点配置文件，将 theme 字段的值修改为 next。

> _config.yml
```
theme: next
```
这个时候刷新浏览器页面并不会发生变化，需要重启服务器并刷新才能使主题生效。

![tvQYJH.jpg](https://s1.ax1x.com/2020/06/13/tvQYJH.jpg)

<center>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px;">Hexo next主题</div>
</center>

```
如果重启服务器仍无效，尝试使用 hexo clean 清除缓存
```

Next 默认主题风格为 Muse，用户可以在主题配置文件中修改 scheme 字段以选择自己喜欢的主题风格：

> themes\next\_config.yml
```
# Schemes
scheme: Muse
#scheme: Mist
#scheme: Pisces
#scheme: Gemini
```

### 三、站点优化
#### 1.完善站点基础信息
在站点配置文件中完善网站基本信息：

> _config.yml
```
title: Yearito's Blog  # 站点名称
description: Stay hungry, stay foolish.  # 站点描述
language: zh-CN # 设置网站语言为简体中文
author: yearito  # 作者名称
```
#### 2.修改站点页脚
在主题配置文件中修改网站页脚信息：

> themes\next\_config.yml

```
footer:  # 底部信息区
  since: 2018  # 建站时间
  icon:
    name: heart   # 图标名称
    animated: true   # 开启动画
    color: "#ff0000"   # 图标颜色

  powered:
    enable: true  # 显示由 Hexo 强力驱动
    version: false  # 隐藏 Hexo 版本号

  theme:
    enable: true  # 显示所用的主题名称
    version: false  # 隐藏主题版本号
```

#### 3.取消数字编号
在主题配置文件中关闭目录中的数字编号：

> themes\next\_config.yml
```
toc:
  number: false  # 关闭目录中的数字编号
```