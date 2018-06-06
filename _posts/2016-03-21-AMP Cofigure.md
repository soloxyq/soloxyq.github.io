title: "AMP"
date: 2016-03-21 16:33:12
tags:
- AMP
- WAMP
categories: soft_use
description: win10下配置AMP
feature: /images/mainmenu/141.jpg
toc: true
---
#### 0x00 我的软件环境 
Win10.0.10586
#### 0x01 Downloads
##### Decide Your Applicatio Spec
- 32位 or 64位

##### Download Prepare
* httpd-2.4.20-x86-vc14.zip
* mysql-5.7.11-win32.zip
* php-7.0.5-Win32-VC14-x86.zip
* phpMyAdmin-4.6.0-all-languages.zip

##### Related Web Sites
* [Using Apache HTTP Server on Microsoft Windows](http://httpd.apache.org/docs/current/en/platform/windows.html)
* [httpd-2.4.20-x86-vc14.zip](http://www.apachehaus.com/cgi-bin/download.plx)
* [vc_redist.x86.exe](https://www.microsoft.com/zh-CN/download/details.aspx?id=48145)
* [mysql-5.7.11-win32.zip](http://dev.mysql.com/downloads/mysql/)
* [php-7.0.5-Win32-VC14-x86.zip](http://windows.php.net/downloads/releases/php-7.0.5-Win32-VC14-x86.zip)
* [phpMyAdmin-4.6.0-all-languages.zip](https://files.phpmyadmin.net/phpMyAdmin/4.6.0/phpMyAdmin-4.6.0-all-languages.zip)
<!-- more -->

#### 0x02 AMP Config
##### Windows Services Command
```bash
httpd  –k install
net start apache2.4
```
##### Apache2
conf/httpd.conf
```bash
ServerRoot
DocumentRoot 
Listen 127.0.0.1:80

PHP扩展
PHPIniDir "D:/hackPHP/workspace/php-7.0.5-Win32-VC14-x86"
LoadModule php7_module "D:/hackPHP/workspace/php-7.0.5-Win32-VC14-x86/php7apache2_4.dll"
<IfModule mime_module>
	TypesConfig conf/mime.types
	AddType application/x-httpd-php .php
</IfModule>		

DirectoryIndex index.php index.html
```
{% label 注意虚拟机共享功能占用443端口. warning %}
{% label 我的花生壳(bearxuqian.oicp.net) warning %}
##### MYSQL
* ADD PATH D:\hackPHP\workspace\mysql-5.7.11-win32\bin
* my.ini 
```bash
[mysqld]
# skip-grant-tables
basedir=D:\hackPHP\workspace\mysql-5.7.11-win32
datadir=D:\hackPHP\workspace\mysql-5.7.11-win32\data
```
* mysql init
```bash
mysqld --initialize --user=mysql --console
VC?auflIC0J0                                                                    #生成默认密码
mysqld.exe -install
net start mysql
mysql -u root -pVC?auflIC0J0
set password for root@localhost = password('passroot');                         #修改密码
mysql -u root -ppassroot
```
##### PHP7
php.ini-development ---> php.ini
```bash
; On windows:
extension_dir = "D:/hackPHP/workspace/php-7.0.5-Win32-VC14-x86/ext"

[opcache]
zend_extension=D:\hackPHP\workspace\php-7.0.5-Win32-VC14-x86\ext\php_opcache.dll
opcache.enable=1
opcache.enable_cli=1
opcache.memory_consumption=128
opcache.interned_strings_buffer=8
opcache.max_accelerated_files=4000
opcache.revalidate_freq=60
opcache.fast_shutdown=1
```
##### phpMyAdmin
config.inc.php
php.ini
```
extension=php_bz2.dll
extension=php_mbstring.dll
extension=php_mysqli.dll
```