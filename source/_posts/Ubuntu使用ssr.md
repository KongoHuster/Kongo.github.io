---
title: Ubuntu使用ssr
date: 2020-06-22 16:48:42
categories: Linux
tags:
- Linux
---

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="Ubuntu使用ssr/n6r6m7.jpg">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px;">深邃眼眸</div>
</center>

#### 一、下载
下载electron-ssr
```
https://github.com/shadowsocksrr/electron-ssr/releases
```
安装
```
sudo dpkg -i <包名>
```
<!-- more -->

#### 二、配置终端

先点击图标启动electron-ssr，将自己的ssr配置添加进去

终端临时走ssr需配置
```
$ export http_proxy="http://127.0.0.1:12333"
$ export https_proxy="https://127.0.0.1:12333"
```
检测是否可以翻墙
```
curl -i www.google.com
```