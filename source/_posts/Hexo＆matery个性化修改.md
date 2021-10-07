---
title: Hexo＆matery个性化修改
date: 2021-10-07 22:11:19
tags:
categories: 
    - Hexo
    - matery
---
#### 修改主题配色
配色包括导航栏，底部栏，a标签等。原主题配色是绿色，但是又不想用黑色调，又不知道什么色调好看，随便选俩颜色渐变吧。
##### 1.修改`themes\Matery\source\css\matery.css`样式
快捷键在文件中搜索.bg-color选择器，修改为 `#005678` 渐变到 `#ffabcd`（不知道什么色，随便输的16进制）
##### 2.取消首页渐变色动画
在`themes\Matery\source\css\matery.css`，ctrl+F快捷键查找`.bg-cover:after`，注释掉即可。
~~~ css
/*.bg-cover:after {*/
/*    -webkit-animation: rainbow 60s infinite;*/
/*    animation: rainbow 60s infinite;*/
/*}*/
~~~
##### 3.修改背景图
在模板配置文件_config.yml中搜索 background `修改enable为true 修改url`，我这里用的是网络图片，大家也可以用[随机图片的api](https://source.unsplash.com/random/1920*1080)，将url替换为api地址即可，api地址可以自行百度。
~~~ yaml
background:
  enable: true
  url: "http://dl.ppt123.net/pptbj/51/20181115/l4gyylcnf10.jpg"
~~~
##### 4.修改banner图
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
打开文件`materialize.min.css` 修改`.red选择器`中的背景颜色为其他颜色或者替换为图片，我这里知己替换为图片了。这也就意味着前面对banner的修改失效了，可以选择注释掉
![](https://p.pstatp.com/origin/pgc-image/8a3afc11a3824200a0b180ac8a8eb336)