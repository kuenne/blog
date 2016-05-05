---
layout: post
title: 解决atom插件无法安装
category : atom
tagline: "Supporting tagline"
tags : [atom]
---
{% include JB/setup %}
# 解决atom插件无法安装
---

修改~user/.atom/.apmrc文件，没有的话新建。

添加 ```registry = https://registry.npm.taobao.org```

使用淘宝的npm镜像安装。

