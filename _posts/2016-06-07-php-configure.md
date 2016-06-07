---
layout: post
title: configure: error: Don’t know how to define struct flock on this system, set –enable-opcache=no
category : atom
tagline: "Supporting tagline"
tags : [atom]
---
{% include JB/setup %}
# configure: error: Don’t know how to define struct flock on this system, set –enable-opcache=no
---

```

vim /etc/ld.so.conf.d/local.conf

```

添加:

```
/usr/local/lib
/usr/local/lib64
/usr/lib
/usr/lib64
/usr/local/mysql/lib
```

最后使生效:

```
ldconfig -v
```