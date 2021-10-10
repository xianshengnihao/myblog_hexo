---
title: 添加Rss订阅
tags: Rss
categories: Hexo
abbrlink: ebe2c743
date: 2021-10-07 21:02:25
---
### 安装feed插件
1.输入指令: `npm install hexo-generator-feed` 
2.等待安装完成

### 添加配置文件
#### 1.打开hexo目录下配置文件_config.yml，末尾添加以下配置

``` bash
# Extensions
## Plugins: http://hexo.io/plugins/
#RSS订阅
plugin:
- hexo-generator-feed
#Feed Atom
feed:
type: atom
path: atom.xml
limit: 20
```
- **type**: RSS的类型(atom/rss2)
- **path**: 文件路径，默认是 atom.xml/rss2.xml
- **limit**: 展示文章的数量,使用 0 或则 false 代表展示全部
- **hub**: URL of the PubSubHubbub hubs (如果使用不到可以为空)
- **content**: （可选）设置 true 可以在 RSS 文件中包含文章全部内容，默认：false
- **content_limit**: （可选）摘要中使用的帖子内容的默认长度。 仅在内容设置为false且未显示自定义帖子描述时才使用。
- **content_limit_delim**: （可选）If content_limit is used to shorten post contents, only cut at the last occurrence of this delimiter before reaching the character limit. Not used by default.
- **order_by**: 订阅内容的顺序. (默认: -date)

#### 2.打开主题配置文件_config.yml
添加配置:`rss: /atom.xml`
#### 3.随后重新生成博客静态文件
- hexo clean
- hexo g
- hexo s --debug