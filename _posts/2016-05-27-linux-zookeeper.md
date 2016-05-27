---
layout: post
title: centos搭建zookeeper
category : zookeeper
tagline: "Supporting tagline"
tags : [centos,linux,zookeeper]
---
{% include JB/setup %}
# centos搭建zookeeper
---

服务器版本：CentOS release 6.5 (Final)

**一.下载Zookeeper最新版本  地址<a href='http://www.apache.org/dist/zookeeper/'>http://www.apache.org/dist/zookeeper/</a>,并解压**

**二.zookeeper-3.4.8/conf 下有一个文件zoo_sample.cfg的这个文件里面配置了监听客户端连接的端口等一些信息**


```
cp zoo_sample.cfg zoo.cfg

```

配置文件参数：


```
clientPort：监听客户端连接的端口。

tickTime：基本事件单元，以毫秒为单位。它用来控制心跳和超时，默认情况下最小的会话超时时间为两倍的 tickTime。

maxClientCnxns：限制连接到 ZooKeeper 的客户端的数量
```

<!--break-->

**三.设置开机启动**

1.编写运行脚本：

```
cd /etc/init.d/
vi zookeeper
```

```
#!/usr/bin/env bash
# chkconfig:    2345 80 90
# description:  zookeeper
export JAVA_HOME=/usr/local/jdk/jdk1.7.0_45
export PATH=$JAVA_HOME/bin:$PATH
case $1 in

  start) su root /home/soft/zookeeper-3.4.8/bin/zkServer.sh start   ;;
  stop) su root /home/soft/zookeeper-3.4.8/bin/zkServer.sh stop;;
  status) su root /home/soft/zookeeper-3.4.8/bin/zkServer.sh status;;
  restart) su root /home/soft/zookeeper-3.4.8/bin/zkServer.sh restart;;
  *)  echo "require start|stop|status|restart"  ;;

esac
```

```
chkconfig --add zookeeper
```

**四.配置日志目录：**

```
vi /etc/profile
添加:
ZOO_LOG_DIR="/home/soft/zookeeper-3.4.8/log"
export ZOO_LOG_DIR

souce /etc/profile
```

**五.启动服务：**

```
service zookeeper start

service zookeeper status
ZooKeeper JMX enabled by default
Using config: /home/soft/zookeeper-3.4.8/bin/../conf/zoo.cfg
Mode: standalone
以上表示启动成功
```

PS:如未启动成功，请查看脚本权限