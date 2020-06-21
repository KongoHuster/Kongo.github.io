---
title: 三、Hexo 搭建个人博客系列：主题美化篇
date: 2020-06-20 15:42:16
tags:
- Hexo
---

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://s1.ax1x.com/2020/06/20/Nl8KL8.jpg">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px;">幻彩魔方</div>
</center>

#### 一、修改博客字体
在 Google Fonts 上找到心仪的字体，然后在主题配置文件中为不同的应用场景配置字体：
<!-- more -->
> themes\next\_config.yml
```
font:
  enable: true

  # 外链字体库地址，例如 //fonts.googleapis.com (默认值)
  host:

  # 全局字体，应用在 body 元素上
  global:
    external: true
    family: Monda

  # 标题字体 (h1, h2, h3, h4, h5, h6)
  headings:
    external: true
    family: Roboto Slab

  # 文章字体
  posts:
    external: true
    family:

  # Logo 字体
  logo:
    external: true
    family:

  # 代码字体，应用于 code 以及代码块
  codes:
    external: true
    family:
```

#### 二、文章页末美化

##### 为标签添加图标
默认情况下标签前缀是 # 字符，用户可以通过修改主题源码将标签的字符前缀改为图标前缀

在文章布局模板中找到文末标签相关代码段，将 # 换成 <i class="fa fa-tags"></i> 即可：

> themes\next\layout\_macro\post.swig
```
  <footer class="post-footer">
    {% if post.tags and post.tags.length and not is_index %}
      <div class="post-tags">
        {% for tag in post.tags %}
-          <a href="{{ url_for(tag.path) }}" rel="tag"># {{ tag.name }}</a>
+          <a href="{{ url_for(tag.path) }}" rel="tag"><i class="fa fa-tags"></i> {{ tag.name }}</a>
        {% endfor %}
      </div>
    {% endif %}
    ...
  </footer>
```

##### 添加结束标记

新建布局模板文件 post-end-tag.swig，添加如下代码：

> themes\next\layout\_macro\post-end-tag.swig
```
<div>
  {% if not is_index %}
    <div style="text-align:center;color:#bfbfbf;font-size:16px;">
      <span>-------- 本文结束 </span>
      <i class="fa fa-{{ config.post_end_tag.icon }}"></i>
      <span> 感谢阅读 --------</span>
    </div>
  {% endif %}
</div>
```
在文章布局模板中添加如下代码：

> themes\next\layout\_macro\post
```
{#####################}
{### END POST BODY ###}
{#####################}

+ {% if config.post_end_tag.enabled and not is_index %}
+   <div>
+     {% include 'post-end-tag.swig' %}
+   </div>
+ {% endif %}

{% if theme.wechat_subscriber.enabled and not is_index %}
  <div>
    {% include 'wechat-subscriber.swig' %}
  </div>
{% endif %}
```
在站点配置文件末尾添加如下代码：

> _config.yml
```
post_end_tag:
  enabled: true  # 是否开启文末的本文结束标记
  icon: paw # 结束标记之间的图标
```
重启服务器后即可在文末看到结束标记。

#### 三、页面加载进度条
当网络不好的时候可能会在打开站点或跳转文章时出现短暂的白屏，此时如果能有加载进度提示将会提高用户操作体验。

在根目录下执行以下命令安装相关依赖：

```
$ git clone https://github.com/theme-next/theme-next-pace themes/next/source/lib/pace
```
在主题配置文件中设置 pace: true。

默认提供了多种主题的进度条加载样式，有顶部提示的，有中间提示的，还有全页面遮挡提示的，个人认为默认的进度条效果就恰如其当，既能够在页面空白的时候起到加载作用，也不会因为太过花里胡哨而喧宾夺主，尤其是当你如果使用了不蒜子的站点访问统计的功能的时候，常常会遇到所有资源都加载完毕而不蒜子还在等待响应，如果这个时候在页面较显眼的位置出现一个停滞不前的进度条，很让人抓狂。

#### 四、添加动态背景
Next 主题可以通过安装插件快速为站点添加不同效果的动态背景。

粒子漂浮聚合

该功能由 theme-next-canvas-nest 插件提供，在根目录下执行如下命令：

```
$ git clone https://github.com/theme-next/theme-next-canvas-nest themes/next/source/lib/canvas-nest
```
然后在主题配置文件中设置 canvas_nest: true 即可。

Next v6.5.0 及以上版本支持更多的自定义选项：

> themes\next\_config.yml
```
canvas_nest:
  enable: true
  onmobile: true # 是否在移动端显示
  color: '0,0,255' # 动态背景中线条的 RGB 颜色
  opacity: 0.5 # 动态背景中线条透明度
  zIndex: -1 # 动态背景的 z-index 属性值
  count: 99 # 动态背景中线条数量
```

#### 五、添加看板娘

在站点根目录下执行以下命令安装依赖：

```
$ npm install --save hexo-helper-live2d
```
在站点配置文件中添加以下下配置项

> _config.yml
```
# Live2D
# https://github.com/EYHN/hexo-helper-live2d
live2d:
  enable: true
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/ Relative)

  # 脚本加载源
  scriptFrom: local # 默认从本地加载脚本
  # scriptFrom: jsdelivr # 从 jsdelivr CDN 加载脚本
  # scriptFrom: unpkg # 从 unpkg CDN 加载脚本
  # scriptFrom: https://cdn.jsdelivr.net/npm/live2d-widget@3.x/lib/L2Dwidget.min.js # 从自定义地址加载脚本
  tagMode: false # 只在有 {{ live2d() }} 标签的页面上加载 / 在所有页面上加载
  log: false # 是否在控制台打印日志

  # 选择看板娘模型
  model:
    use: live2d-widget-model-shizuku  # npm package的名字
    # use: wanko # /live2d_models/ 目录下的模型文件夹名称
    # use: ./wives/wanko # 站点根目录下的模型文件夹名称
    # use: https://cdn.jsdelivr.net/npm/live2d-widget-model-wanko@1.0.5/assets/wanko.model.json # 自定义网络数据源
  display:
    position: left # 显示在左边还是右边
    width: 100 # 宽度
    height: 180 # 高度
  mobile:
    show: false
  react:
    opacityDefault: 0.7 # 默认透明度
```

此时重启服务器暂时还看不到看板娘，需要手动下载或安装模型资源。可以从 [hexo live2d](https://huaji8.top/post/live2d-plugin-2.0/) 模型预览 里找到你喜欢的角色，然后根据 live2d-widget-models 中提供的方法来下载模型数据.

例如通过以下命令下载模型 shizuku：
```
$ npm install live2d-widget-model-shizuku
```
因为修改了站点配置文件，所以需要重启服务器才能预览模型效果。

如果设置了 live2d.tagMode: true，则可以在指定页面中插入以下标签：

```
{{ live2d() }}
```
只有拥有该标签的页面才会渲染 live2d 模型，这样以来就可以精确控制在哪些页面上显示看板娘了。

如果只想在一级菜单页面上显示看板娘，可以在 Header 模板中添加以下代码：

> themes\next\layout\_partials\header\index.swig
```
+ {% if is_index %}
+   {{ live2d() }}
+ {% endif %}
```

#### 六、个性化回到顶部

原理很简单，将 back-to-top 按钮添加图片背景，并添加 CSS3 动效即可。

首先，找到自己喜欢的图片素材放到 source\images\ 目录下。

你可以点击下方按钮下载本站所使用的小猫上吊素材（ 小猫咪这么可爱，当然要多放点孜然啦…）

[下载图片](http://yearito.cn/images/scroll.png)
 
然后在自定义样式文件中添加如下代码：

> themes\next\source\css\ _custom\custom.styl
```
//自定义回到顶部样式
.back-to-top {
  right: 60px;
  width: 70px;  //图片素材宽度
  height: 900px;  //图片素材高度
  top: -900px;
  bottom: unset;
  transition: all .5s ease-in-out;
  background: url("/images/scroll.png");

  //隐藏箭头图标
  > i {
    display: none;
  }

  &.back-to-top-on {
    bottom: unset;
    top: 100vh < (900px + 200px) ? calc( 100vh - 900px - 200px ) : 0px;
  }
}
```
刷新浏览器即可预览效果。

#### 七、鼠标点击特效

点击下方按钮下载相应的脚本，并置于 themes\next\source\js\cursor\ 目录下：

[下载](https://script-1256884783.file.myqcloud.com/cursor/fireworks.js)

在主题自定义布局文件中添加以下代码：

> themes\next\layout\_custom\custom.swig
```
{# 鼠标点击特效 #}
{% if theme.cursor_effect == "fireworks" %}
  <script async src="/js/cursor/fireworks.js"></script>
{% elseif theme.cursor_effect == "explosion" %}
  <canvas class="fireworks" style="position: fixed;left: 0;top: 0;z-index: 1; pointer-events: none;" ></canvas>
  <script src="//cdn.bootcss.com/animejs/2.2.0/anime.min.js"></script>
  <script async src="/js/cursor/explosion.min.js"></script>
{% elseif theme.cursor_effect == "love" %}
  <script async src="/js/cursor/love.min.js"></script>
{% elseif theme.cursor_effect == "text" %}
  <script async src="/js/cursor/text.js"></script>
{% endif %}
```
如果 custom.swig 文件不存在，需要手动新建并在布局页面中 body 末尾引入：
```
themes\next\layout\_layout.swig
      ...
      {% include '_third-party/exturl.swig' %}
      {% include '_third-party/bookmark.swig' %}
      {% include '_third-party/copy-code.swig' %}

+     {% include '_custom/custom.swig' %}
    </body>
  </html>
```
在主题配置文件中添加以下代码：

> themes\next\_config.yml
```
# mouse click effect: fireworks | explosion | love | text
cursor_effect: fireworks
```
这样即可在配置文件中一键快速切换鼠标点击特效。