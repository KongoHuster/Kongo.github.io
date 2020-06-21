---
title: Hexo 搭建个人博客系列：进阶设置篇
categories: Hexo
tags: Hexo
date: 2020-06-21 14:30:04
---
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="Hexo-搭建个人博客系列：进阶设置篇/星辰.jpg">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px;">醉美星辰</div>
</center>


#### 一、后台管理界面 
<!-- more -->
安装插件
```
$ npm install --save hexo-admin
```
浏览器访问localhost:4000/admin

#### 二、站点访问量统计
在页脚布局模板文件首行添加如下代码：

> themes\next\layout_partial\footer.swig
```
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js">
</script>
```
在文件末尾添加

```
<br>
Total <span id="busuanzi_value_site_pv"></span> views.
<br>
您是闳的第<span id="busuanzi_value_site_uv"></span>个小伙伴
<br>
<span id="busuanzi_value_page_pv"></span> Hits
```

#### 三、