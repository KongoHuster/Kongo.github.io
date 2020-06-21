---
title: 四、Hexo 搭建个人博客系列：进阶设置篇
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

#### 三、站点及文章字数统计

在根目录下执行如下命令安装相关依赖
```
$ npm install hexo-symbols-count-time --save
```
启用该功能需要同时修改站点配置文件和主题配置文件。

将如下配置项添加到站点配置文件中，这些配置项主要用于控制每项统计信息是否显示。

> _config.yml
```
symbols_count_time:
  symbols: true # 统计单篇文章字数
  time: false # 取消估算单篇文章阅读时间
  total_symbols: true # 统计站点总字数
  total_time: false # 取消估算站点总阅读时间
```
在主题配置文件中做如下修改，这些配置项主要用于控制统计信息的显示样式。

> themes\next\ _config.yml
```
symbols_count_time:
  separated_meta: false # 统计信息不换行显示
  item_text_post: true # 文章统计信息中是否显示“本文字数/阅读时长”等描述文字
  item_text_total: true # 站点统计信息中是否显示“本文字数/阅读时长”等描述文字
  awl: 4 # Average Word Length：平均字符长度
  wpm: 275 # Words Per Minute：阅读速度
```
汉字的平均字符长度为 1.5，如果在文章中使用纯中文进行写作（没有混杂英文），那么推荐设置 awl: 2 及 wpm: 300，但是如果文章中存在英文，建议设置 awl: 4 及 wpm: 275。

#### 四、添加评论功能

Next 已经内置了 Valine 组件，在主题配置文件中开启评论功能即可，同时，由于 Valine 是基于 Leancloud 提供后端服务的，所以需要填写 LeanCloud 的 App ID 和 App Key。

> themes\next_config.yml
```
valine:
  enable: true
  appid:  ***<app_id***
  appkey: ***<app_key>***
  notify: false  # 收到新评论是否邮件通知
  verify: false  # 是否开启验证码
  placeholder:  # 默认填充文字
  avatar: mm  # 设置默认评论列表
  guest_info: nick,mail  # 评论区头部表单
  pageSize: 10  # 每页评论数
  visitor: true  # 同时开启文章阅读次数统计
```

#### 五、添加打赏功能

启用主题配置文件中的打赏相关字段，并将个人收款码图片置于 themes\next\source\images\ 目录下，注意保持图片命名与配置文件中一致：

> themes\next_config.yml
```
reward_settings:
  # If true, reward will be displayed in every article by default.
  enable: true
  animation: true
  comment: 你的支持就是我前进的动力

reward:
  #wechatpay: /images/wechatpay.png
  #alipay: /images/alipay.png
  #paypal: /images/paypal.png
  #bitcoin: /images/bitcoin.png
  wechatpay: /images/wechatpay.jpg
  alipay: /images/alipay.jpg  
```

#### 六、添加图片灯箱
添加灯箱功能，实现点击图片后放大聚焦图片，并支持幻灯片播放、全屏播放、缩略图、快速分享到社交媒体等，该功能由 fancyBox 提供
在根目录下执行以下命令安装相关依赖：
```
git clone https://github.com/theme-next/theme-next-fancybox3 themes/next/source/lib/fancybox
```
在主题配置文件中设置 fancybox: true：

> themes\next_config.yml
```
fancybox: true
```

#### 七、文章加密访问
该功能由 hexo-blog-encrypt 插件提供。

在站点根目录中执行以下命令安装依赖：
```
$ npm install hexo-blog-encrypt --save
```
在站点配置文件中添加如下字段：

> _config.yml
```
encrypt:
  enable: true
  default_abstract: 此文章已被加密，需要输入密码访问。  //首页文章列表中加密文章的默认描述文案
  default_message: 请输入密码以阅读这篇私密文章。  //文章详情页的密码输入框上的默认描述文案
```
然后在文章 Front-Matter 中添加 password 字段用于设置文章访问密码。重启服务器，这个时候可能需要经历较长一段时间的加密过程，请耐心等待，加密完成后刷新页面将会显示密码输入框，输入密码后才能继续访问文章内容。