---
title: hexo plugin 插件集成
date: 2020-03-03 15:24:33
tags: hexo
author: 相飞
comments:
- true
categories:
- hexo
- plugin
- markdown
---



###  hexo-theme-matery 新插件集成


#### 插件安装


```
npm install --save hexo-tag-mermaid

```


#### hexo 配置

- #{hexo_project_root}/_config.yml 


```
hexo-tag-mermaid:
  enable: true

```


####  hexo-theme-matery
- 上传静态文件夹
  - #{hexo-theme-matery}/source/libs
- 修改文件
  - #{hexo-theme-matery}/_config.yml

```
libs:
  css:
    fontAwesome: /libs/awesome/css/font-awesome.min.css # V4.7.0
    materialize: /libs/materialize/materialize.min.css # 1.0.0
    aos: /libs/aos/aos.css
    animate: /libs/animate/animate.min.css # V3.5.1
    lightgallery: /libs/lightGallery/css/lightgallery.min.css # V1.6.11
    aplayer: /libs/aplayer/APlayer.min.css
    dplayer: /libs/dplayer/DPlayer.min.css
    gitalk: /libs/gitalk/gitalk.css
    jqcloud: /libs/jqcloud/jqcloud.css
    tocbot: /libs/tocbot/tocbot.css
    mermaid: /libs/mermaid/mermaid.min.css
  js:
    jquery: /libs/jquery/jquery-2.2.0.min.js
    materialize: /libs/materialize/materialize.min.js # 1.0.0
    masonry: /libs/masonry/masonry.pkgd.min.js # v4.0.0
    aos: /libs/aos/aos.js
    scrollProgress: /libs/scrollprogress/scrollProgress.min.js
    lightgallery: /libs/lightGallery/js/lightgallery-all.min.js # V1.6.11
    clicklove: /libs/others/clicklove.js
    busuanzi: /libs/others/busuanzi.pure.mini.js
    aplayer: /libs/aplayer/APlayer.min.js
    dplayer: /libs/dplayer/DPlayer.min.js
    crypto: /libs/cryptojs/crypto-js.min.js
    echarts: /libs/echarts/echarts.min.js
    gitalk: /libs/gitalk/gitalk.min.js
    jqcloud: /libs/jqcloud/jqcloud-1.0.4.min.js
    tocbot: /libs/tocbot/tocbot.min.js
    mermaid: /libs/mermaid/mermaid.js

```
