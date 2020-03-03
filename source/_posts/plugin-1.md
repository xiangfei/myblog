---
title: hexo-theme-matery 增加新link
date: 2020-03-03 15:34:34
tags: hexo
author: 相飞
comments:
- true
categories:
- hexo
- plugin
- markdown

---



##  hexo-theme-matery 增加新link

#### step 1
 hexo-theme-matery/layout/_partial/navigation.ejs 
 hexo-theme-matery/layout/_partial/mobile-nav.ejs

```
<%
    var menuMap = new Map();
    menuMap.set("Index", "首页");
    menuMap.set("Tags", "标签");
    menuMap.set("Categories", "分类");
    menuMap.set("Archives", "归档");
    menuMap.set("About", "关于");
    menuMap.set("Friends", "朋友");
    menuMap.set("Studies", "学习资料");
%>

```
#### step2
- add new file
  - themes/hexo-theme-matery/layout/studies.ejs

```
 文件模板直接copy ,基于friends
 修改
<main class="content">
    <div class="container friends-container">
        <article>
            <% if (site.data && site.data.studies) { %>
            <% var friends = site.data.studies; %>
            <div class="row tags-posts friend-all">
                <% for (var i = 0, len = friends.length; i < len; i++) { %>
                <% var friend = friends[i]; %>



```



#### steps 3
- 修改文件themes/hexo-theme-matery/languages/zh-CN.yml


```
friends: 朋友
studies: 学习资料 # added

```

#### steps 4

- add #{hexo_root}/source/_data/studies.json 



```

```

#### steps 5 
- ./themes/hexo-theme-matery/_config.yml



```
  Friends:
    url: /friends
    icon: fa-user-circle
  Studies:
    url: /studies
    icon: fa-address-book

```
