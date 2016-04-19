---
layout: post
title: 多平台git账号切换
category : git
tagline: "Supporting tagline"
tags : [git,ssh-key]
---
{% include JB/setup %}
# 多平台git账号切换
---

**1.生成并添加第一个ssh key：**
**第一次使用ssh生成key，默认会在用户~（根目录）下生成 id_rsa, id_rsa.pub 2个文件；所以需要添加多个ssh key时也会生成对应的私钥和公钥。**


```
$ ssh-keygen -t rsa -C "youremail@yourcompany.com"
```
<!--break-->

**2.生成并添加第二个ssh key：**


```
$ ssh-keygen -t rsa -C "youremail@gmail.com"

```

注意不要一路回车，要给这个文件起一个名字， 比如叫 id_rsa_github, 所以相应的也会生成一个 id_rsa_github.pub 文件。

目录结构如下：

**3.修改配置文件：**
在 ~/.ssh 目录下新建一个config文件

添加内容：


```
# gitlab
Host gitlab.com
    HostName gitlab.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa

# github
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_github
```

**4.测试：**
$ ssh -T git@github.com

**ps.  push到github的blog命令: git push origin gh-pages**
