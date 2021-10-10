---
title: Hexo＆matery个性化修改
tags: matery个性化修改
categories:
  - Hexo
  - matery
img: 'https://source.unsplash.com/random/'
top: false
cover: false
summary: 记录matery的一些个性化修改
abbrlink: '21558651'
date: 2021-10-07 22:11:19
keywords:
---
#### 修改主题配色
配色包括导航栏，底部栏，a标签等。原主题配色是绿色，但是又不想用黑色调，又不知道什么色调好看，随便选俩颜色渐变吧。
修改`themes\Matery\source\css\matery.css`样式
快捷键在文件中搜索.bg-color选择器，修改为 `#005678` 渐变到 `#ffabcd`（不知道什么色，随便输的16进制）
#### 取消首页渐变色动画
在`themes\Matery\source\css\matery.css`，ctrl+F快捷键查找`.bg-cover:after`，注释掉即可。
~~~ css
/*.bg-cover:after {*/
/*    -webkit-animation: rainbow 60s infinite;*/
/*    animation: rainbow 60s infinite;*/
/*}*/
~~~
#### 修改背景图
在模板配置文件_config.yml中搜索 background `修改enable为true 修改url`，我这里用的是网络图片，大家也可以用[随机图片的api](https://source.unsplash.com/random/1920*1080)，将url替换为api地址即可，api地址可以自行百度。
~~~ yaml
background:
  enable: true
  url: "http://dl.ppt123.net/pptbj/51/20181115/l4gyylcnf10.jpg"
~~~
#### 修改banner图
我这里直接将主题配置文件_config.yml中banner的每日切换改为false，然后使用的随机图片的api
~~~ yaml
banner:
  enable: false
~~~
修改banner图片使用外部api：修改`themes\Matery\layout\_partial\bg-cover-content.ejs`的js部分
~~~ javascript
<script>
    // 每天切换 banner 图.  Switch banner image every day.
    var bannerUrl = "<%- theme.jsDelivr.url %><%- url_for('/medias/banner/') %>" + new Date().getDay() + '.jpg';
    $('.bg-cover').css('background-image', 'url(' + bannerUrl + ')');
</script>
<% } else { %>
<script>
    $('.bg-cover').css('background-image', 'url(https://source.unsplash.com/random/1920*1080)');
</script>
~~~
修改banner区域使用在随机图片api时，由于加载速度不同显示出红色底色
打开文件`materialize.min.css` 修改`.red选择器`中的背景颜色为其他颜色或者替换为图片，我这里直接替换为图片了。
![](https://p.pstatp.com/origin/pgc-image/8a3afc11a3824200a0b180ac8a8eb336)
#### 增加点击跳转评论按钮
1.在文件目录`themes\Matery\layout\_partial\`下新建`back-comment.ejs`文件,粘贴如下代码
我使用的是minivaline，直接填写的valine的id——href="#mvcomments",如果是其他评论，对应修改即可。
~~~ css
<!-- 直达评论 -->
<div id="to_comment" class="comment-scroll">
    <a class="btn-floating btn-large waves-effect waves-light" href="#mvcomments" title="直达评论">
        <i class="fas fa-comments"></i>
    </a>
</div>
~~~
2.在themes\Matery\layout\_partial\minivaline.ejs文末添加一条，引用第一步的内容；
~~~ JavaScript
<%- partial('_partial/back-comment.ejs') %>
~~~
3.增加样式在themes\Matery\source\css\matery.css添加内容如下。
~~~ css
/*直达评论按钮样式*/
.comment-scroll {
    position: fixed;
    right: 15px;
    bottom: 135px;
    padding-top: 15px;
    margin-bottom: 0;
    z-index: 998;
}

.comment-scroll .btn-floating {
    background: linear-gradient(to bottom right, #FF9999 0%, #ff6666 100%);
    width: 48px;
    height: 48px;
}

.comment-scroll .btn-floating i {
    line-height: 48px;
    font-size: 1.8rem;
}
~~~ 
`bottom: 135px;`是距离底部的高度，看看你是否需要修改。
#### 修改滑动条
在`themes\Matery\source\css\matery.css`样式添加如下：

~~~ css
/* 滚动条 */
::-webkit-scrollbar-thumb {
    background-color: #448AFF;
    background-image: -webkit-linear-gradient(45deg,rgba(255,255,255,.4) 25%,transparent 25%,transparent 50%,rgba(255,255,255,.4) 50%,rgba(255,255,255,.4) 75%,transparent 75%,transparent);
    border-radius: 3em;
}
::-webkit-scrollbar-track {
    background-color: #ffabcd;
    border-radius: 3em;
}
::-webkit-scrollbar {
    width: 4px;
    height: 10px;
}
~~~
#### Front-matter 选项详解

`Front-matter` 选项中的所有内容均为**非必填**的。但我仍然建议至少填写 `title` 和 `date` 的值。

| 配置选项   | 默认值                      | 描述                                                         |
| ---------- | --------------------------- | ------------------------------------------------------------ |
| title      | `Markdown` 的文件标题        | 文章标题，强烈建议填写此选项                                 |
| date       | 文件创建时的日期时间          | 发布时间，强烈建议填写此选项，且最好保证全局唯一             |
| author     | 根 `_config.yml` 中的 `author` | 文章作者                                                     |
| img        | `featureImages` 中的某个值   | 文章特征图，推荐使用图床(腾讯云、七牛云、又拍云等)来做图片的路径.如: `http://xxx.com/xxx.jpg` |
| top        | `true`                      | 推荐文章（文章是否置顶），如果 `top` 值为 `true`，则会作为首页推荐文章 |
| cover      | `false`                     | `v1.0.2`版本新增，表示该文章是否需要加入到首页轮播封面中 |
| coverImg   | 无                          | `v1.0.2`版本新增，表示该文章在首页轮播封面需要显示的图片路径，如果没有，则默认使用文章的特色图片 |
| password   | 无                          | 文章阅读密码，如果要对文章设置阅读验证密码的话，就可以设置 `password` 的值，该值必须是用 `SHA256` 加密后的密码，防止被他人识破。前提是在主题的 `config.yml` 中激活了 `verifyPassword` 选项 |
| toc        | `true`                      | 是否开启 TOC，可以针对某篇文章单独关闭 TOC 的功能。前提是在主题的 `config.yml` 中激活了 `toc` 选项 |
| mathjax    | `false`                     | 是否开启数学公式支持 ，本文章是否开启 `mathjax`，且需要在主题的 `_config.yml` 文件中也需要开启才行 |
| summary    | 无                          | 文章摘要，自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要 |
| categories | 无                          | 文章分类，本主题的分类表示宏观上大的分类，只建议一篇文章一个分类 |
| tags       | 无                          | 文章标签，一篇文章可以多个标签                              |
| keywords   | 文章标题                     | 文章关键字，SEO 时需要                              |
| reprintPolicy | cc_by                    | 文章转载规则， 可以是 cc_by, cc_by_nd, cc_by_sa, cc_by_nc, cc_by_nc_nd, cc_by_nc_sa, cc0, noreprint 或 pay 中的一个 |

> **注意**:
> 1. 如果 `img` 属性不填写的话，文章特色图会根据文章标题的 `hashcode` 的值取余，然后选取主题中对应的特色图片，从而达到让所有文章的特色图**各有特色**。
> 2. `date` 的值尽量保证每篇文章是唯一的，因为本主题中 `Gitalk` 和 `Gitment` 识别 `id` 是通过 `date` 的值来作为唯一标识的。
> 3. 如果要对文章设置阅读验证密码的功能，不仅要在 Front-matter 中设置采用了 SHA256 加密的 password 的值，还需要在主题的 `_config.yml` 中激活了配置。有些在线的 SHA256 加密的地址，可供你使用：[开源中国在线工具](http://tool.oschina.net/encrypt?type=2)、[chahuo](http://encode.chahuo.com/)、[站长工具](http://tool.chinaz.com/tools/hash.aspx)。
> 4. 您可以在文章md文件的 front-matter 中指定 reprintPolicy 来给单个文章配置转载规则

以下为文章的 `Front-matter` 示例。

##### 最简示例

```yaml
---
title: typora-vue-theme主题介绍
date: 2018-09-07 09:25:00
---
```

##### 最全示例

```yaml
title: typora-vue-theme主题介绍
date: 2018-09-07 09:25:00
author: 赵奇
img: /source/images/xxx.jpg
top: true
cover: true
coverImg: /images/1.jpg
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: false
mathjax: false
summary: 这是你自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要
categories: Markdown
tags:
  - Typora
  - Markdown
```
## 未完待续~~~~