---
layout: post
title: 线上服务器php版本升级
category : git
tagline: "Supporting tagline"
tags : [php,linux]
---
{% include JB/setup %}
# 线上服务器php版本升级
---

服务器版本：CentOS release 6.5 (Final)

php版本从5.2升级到5.6

**1.把 /usr/local/php 重命名 以防止如果新版本更新失败 回滚**

```
mv /usr/local/php  /usr/local/php5.2
```
<!--break-->

**2.下载最新版本php5.6.20 并且解压：**


```
wget  http://www.php.net/distributions/php-5.6.20.tar.gz  
tar -zxvf  php-5.6.20.tar.gz

```

configure 配置参数


```
./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-fpm-user=www --with-fpm-group=www --enable-fpm --enable-opcache --with-mysql=/usr/local/mysql --with-mysqli=/usr/local/mysql/bin/mysql_config --with-pdo-mysql=/usr/local/mysql --disable-fileinfo --with-iconv-dir --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-exif --enable-sysvsem --enable-inline-optimization --with-curl=/usr/local/curl --enable-mbregex --enable-mbstring --with-mcrypt --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-ftp --with-gettext --enable-zip --enable-soap --disable-ipv6 --disable-debug
```

其中开启opcache，可以缓存opcode提高php性能

还有 --disable-debug --disable-ipv6  关闭 debug ipv6  可以提升性能

第一次make出错:

```
/usr/local/php-5.2.13/ext/iconv/iconv.c:2491: undefined reference to `libiconv_open'  
```

解决办法:

```
make clean  #清除上次编译生成的 obj文件  
make ZEND_EXTRA_LIBS='-liconv'  
make install 
```


**3.生成 php-fpm 文件：**

```
vim /usr/local/php/etc/php-fpm.conf  
[global]  
pid = /usr/local/php/logs/php-fpm.pid  
error_log = /usr/local/php/logs/php-fpm.log  
log_level = notice  
  
  
[www]  
listen = /tmp/php-cgi.sock  
listen.backlog = -1  
listen.allowed_clients = 127.0.0.1  
listen.owner = www  
listen.group = www  
listen.mode = 0666  
user = www  
group = www  
pm = dynamic  
pm.max_children = 10  
pm.start_servers = 2  
pm.min_spare_servers = 1  
pm.max_spare_servers = 6  
request_terminate_timeout = 100  
request_slowlog_timeout = 0  
slowlog = var/log/slow.log 
```

**4.添加到系统服务：**

```
cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm  
chmod +x /etc/init.d/php-fpm  
chkconfig --add php-fpm   
chkconfig php-fpm on 
```

**需要修改/etc/init.d/php-fpm的pid路径**

**5.启动服务：**

```
service php-fpm start
/usr/local/nginx/sbin/nginx -s reload
```