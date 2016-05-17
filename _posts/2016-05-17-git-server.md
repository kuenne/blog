---
layout: post
title: Git服务器搭建
category : git
tagline: "Supporting tagline"
tags : [git]
---
{% include JB/setup %}
# Git服务器搭建
---

**1.创建一个git用户，用来运行git服务**

```
adduser git
```

**2.创建证书登录**

收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。

```
cat id_rsa.put >> /home/git/.ssh/authorized_keys
```
<!--break-->

**3.初始化Git仓库**

先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：

```
$ git init --bare sample.git
```

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：

```
$ chown -R git:git sample.git
```

**4.禁用shell登录**
出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

```
git:x:1001:1001:,,,:/home/git:/bin/bash
```

改为：

```
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

**5.克隆远程仓库**

```
$ git clone git@server:/srv/sample.git
Cloning into 'sample'...
warning: You appear to have cloned an empty repository.
```

##管理公钥

如果团队很小，把每个人的公钥收集起来放到服务器的/home/git/.ssh/authorized_keys文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用Gitosis来管理公钥。

这里我们不介绍怎么玩<a href='https://github.com/res0nat0r/gitosis' target='_blank'>Gitosis</a>了，几百号人的团队基本都在500强了，相信找个高水平的Linux管理员问题不大。