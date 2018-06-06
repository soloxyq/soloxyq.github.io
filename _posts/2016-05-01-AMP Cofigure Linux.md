title: "raspbian AMP"
date: 2016-05-01 09:33:12
tags:
- LAMP
- PHP-FPM
- raspbian
categories: soft_use
description: raspberry pi 2配置AMP
feature: /images/mainmenu/102.jpg
toc: true
---
#### 0x00 我的软件环境 
Linux raspberrypi armv7l GNU/Linux	ELF 32-bit LSB
#### 0x01 安装
##### 总结
raspbian os version: 'jessi'

* 由于上一篇在win下使用的php7版本,保持一致.
* 目前的官方源里只有php5,所以需要使用第三方编译好的或者自己来弄一个吧
* 很多人说树莓派自已编译效率太低,但是在虚拟机(双核,2G RAM)中使用交叉编译工具也快不了多少
* mod_php(libapache2-mod-php7.0 --with-apxs2=/usr/bin/apxs2) VS php-fpm(--enable-fpm)
* 127.0.0.1有效 localhost无法连接mysql,有可能是TCP端口连通但是sock配置出问题.

##### apache2
```
sudo apt-get install apache2
```
/etc/apache2/envvars 
```
export APACHE_RUN_USER=www-data
export APACHE_RUN_GROUP=www-data
```
cat /etc/passwd | grep www
```
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
```
##### mysql
```
sudo apt-get install mysql-server 
```
set passwd root = "passroot"
##### php7源码编译
```
sudo apt-get install libcurl-devel libbz2-dev bzip2 libssl-dev libxml2-dev libjpeg-dev libtool-bin libmcrypt-dev 
./configure --prefix=/home/pi/workspace/lamp/php --with-config-file-path=/home/pi/workspace/lamp/php/etc --enable-inline-optimization --disable-debug --disable-rpath --enable-shared --enable-opcache --enable-fpm --with-fpm-user=www-data --with-fpm-group=www-data --with-mysql-sock=/var/run/mysqld/mysqld.sock --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-gettext --enable-mbstring --with-iconv --with-mcrypt --with-mhash --with-openssl --enable-bcmath --enable-soap --with-libxml-dir --enable-pcntl --enable-shmop --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-sockets --with-curl --with-zlib --enable-zip --with-bz2 --with-readline --with-gd --without-pear --enable-xml --with-freetype-dir --with-jpeg-dir --enable-calendar --enable-mbregex --enable-exif --enable-ftp
make -j4 && make install
```
<!-- more -->
#### 0x02 config
```
vi /etc/profile
*export PATH=$PATH:/home/pi/workspace/lamp/php/bin:/home/pi/workspace/lamp/php/sbin
**ln -s /home/pi/workspace/lamp/php/bin/php /usr/bin/php

cd /home/pi/workspace/lamp/php
cp ~/workspace/lamp_install/php-7.0.6/php.ini-production php.ini
sudo cp ~/workspace/lamp_install/php-7.0.6/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
cp php-fpm.conf.default php-fpm.conf
cp www.conf.default www.conf

sudo chmod +x /etc/init.d/php-fpm 
*sudo chkconfig --add php-fpm
*chkconfig php-fpm on
**sudo systemctl enable php-fpm.service
php-fpm -t                                            (测试php-fpm.conf语法)
sudo service php-fpm start
sudo chown www-data:www-data /var/run/php-fpm.sock    (php-fpm运行成功后会生成sock)
```
```
vi php-fpm.conf
pid = run/php-fpm.pid
error_log = log/php-fpm.log
```
```
vi /home/pi/workspace/lamp/php/etc/php-fpm.d/www.conf
--listen = 127.0.0.1:9000
++listen = /var/run/php-fpm.sock
user = www-data
group = www-data
```
apache2 config
```
sudo apt-get install apache2-mpm-worker debhelper dpatch cdbs libapr1-dev apache2-threaded-dev libtool-bin
cd /tmp
apt-get -b source libapache-mod-fastcgi
dpkg -i libapache2-mod-fastcgi*.deb
a2enmod rewrite ssl actions include headers expires fastcgi alias
sudo vi /etc/apache2/mods-available/fastcgi.conf

<IfModule mod_fastcgi.c>
  AddHandler fastcgi-script .fcgi
  #FastCgiWrapper /usr/lib/apache2/suexec

  FastCgiIpcDir /var/lib/apache2/fastcgi
  AddType application/x-httpd-fastphp .php
  Action application/x-httpd-fastphp /php-fcgi
  Alias /php-fcgi /home/pi/workspace/lamp/php/bin/php-cgi
  FastCgiExternalServer /home/pi/workspace/lamp/php/bin/php-cgi -socket /var/run/php-fpm.sock -pass-header Authorization
  <Directory /home/pi/workspace/lamp/php/bin>
    Require all granted
  </Directory>
</IfModule>
```

#### 0x03 test
echo "<?php phpinfo();?>" >> /var/www/html/phpinfo.php

 /var/run/mysqld/mysqld.sock    3306

