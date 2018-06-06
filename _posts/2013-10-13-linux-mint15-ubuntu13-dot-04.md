---
layout: post
title: "linux mint15(raring)"
date: 2013-10-13 18:00
comments: true
categories: skill
feature: /images/mainmenu/yoori.jpg
toc: true
---
### 偶尔整理记录下来,排版是大坑. ###
{% codeblock lang:sh %}
Ubuntu系统架构关于启动项大致分为四类，每一类都分为系统级和用户级	 /home/channe/.dropbox-dist/dropboxd	
第一类upstart，或者叫job，由init管理，配置文件目录/etc/init,~/.init
第二类叫service，由rc.d管理，配置文件目录/etc/init.d，以及/etc/rc.local
第三类叫cron，由contab管理，使用crontab进行配置
第四类叫startup，由xdg管理，配置文件目录/etc/xdg/autostart，以及~/.config/autostart
upstart任务适用于runlevel<5的脚本和程序，service任务适用于runlevel<=5的任务，cron任务则不一定，而startup一般工作在runlevel=5，也就是桌面级。
对于普通用户而言，你的桌面级应用应该使用startup，服务级应用（比如某些功能性的软件脚本）应该使用service，常规性配置可以使用cron，而与启动顺序有关的最好使用upstart.
头文件:			
/usr/src
/usr/lib
/usr/include
lspci |grep VGA
系统环境变量修改/etc/profile，用户环境变量修改~/.profile.
{% endcodeblock %}
<!-- more -->
{% codeblock lang:sh %}
添加osd-lyrics PPA,在/etc/apt/sources.list.d中修改raring--->precise
sudo apt-get install osdlyrics
~/.lyrics
%
/media/channe/softwareinstall/Program Files (x86)/AutoLyric/lyrics
{% endcodeblock %}
### 常用命令 ###
{% codeblock lang:sh %}
####################################ps -aux|grep <进程号>
查询已安装软件包						dpkg --get-selections | grep fcitx
查询该文件属于那个安装包:				dpkg-query -S libGL.so.1
查询系统中属于**的文件:					dpkg --listfiles **
查询端口占用							netstat -apn | grep 127.0.0.1
									lsof -i:8087
{% endcodeblock %}
{% codeblock lang:sh %}
sudo passwd root
{% endcodeblock %}
{% codeblock lang:sh %}
卸载一些程序
	sudo apt-get purge firefox*
	sudo apt-get purge gimp*
	sudo apt-get purge libreoffice-*
	sudo apt-get purge pluma*/gedit*
	sudo apt-get purge totem*					影响 mint-meta-codecs
{% endcodeblock %}
更新语言包language support
upgrade
{% codeblock lang:sh %}
将home目录下的文件名保留为英文
	export LANG=en_US
	xdg-user-dirs-gtk-update
	跳出对话框询问是否将目录转化为英文路径,同意并关闭.
	在终端中输入命令:
	export LANG=zh_CN
{% endcodeblock %}
{% codeblock lang:sh %}
安装一些程序
	fcitx-table-wbpy					/home/channe/.xinputrc 为 fcitx
	system-config-samba
	mate-sensors-applet-ati(lm-sensors)				面板小监控程序
	chrome
	dropbox
	virtualbox
	ubuntu-tweak 0.8.5
	#蛋疼的A卡驱动,再完善的帮助文档也解决不了问题
	驱动参考A卡WIKI,自带足够了. driver-manager --> fglrx-updates
	version 2: 9.012-0ubuntu1
	channe@channe-ubuntu ~ $ fglrxinfo 
	display: :0.0  screen: 0
	OpenGL vendor string: Advanced Micro Devices, Inc.
	OpenGL renderer string: AMD Radeon HD 6500M/5600/5700 Series
	OpenGL version string: 4.2.12002 Compatibility Profile Context 9.012
{% endcodeblock %}
{% codeblock lang:sh %}
配置VIM和GEANY
	vim vim-runtime exuberant-ctags
	geany
	配置直接文件覆盖
	/home/channe/.config/geany
	/home/channe/.vim					～/.vimrc
	#说明geany
	sudo add-apt-repository ppa:geany-dev/ppa 
	sudo apt-get update		
	theme			~/.config/geany/filedefs and restart Geany.
	tags代码提示		~/.config/geany/tags/
	自动补全			~/.config/geany/snippets.conf
	#说明vim插件管理pathogen
	/usr/share/vim/vim73/plugin
	pathogen的下载地址为：https://github.com/tpope/vim-pathogen
	下载后可以直接解压。pathogen插件只有一个单独的脚本，所谓安装就是把它放在当前用户的 ~/.vim/autoload 目录下即可。
	将解压后的autoload目录连同里面的pathogen.vim插件拷贝到~/.vim/目录下。
	如果没有~/.vimrc文件，创建该文件并将以下内容拷贝到该文件中：
	call pathogen#infect()
	syntax on
	filetype plugin indent on
	要生成帮助文档的话，就在vim下输入:call pathogen#helptags()即可。
	要安装新插件，只需要下载该插件，并将其放到~/.vim/bundle/目录下
	eg.
		git clone https://github.com/majutsushi/tagbar.git
		git clone http://github.com/scrooloose/nerdtree.git
		git clone https://github.com/ervandew/supertab.git
		hg clone https://bitbucket.org/abudden/taghighlight
	左边taglist显示函数和变量，右边nerdtree显示源文件列表，omnicppcomplete自动补全，echofunc提示函数原型，再加上半自动的宏/结构体/全局变量高亮。
{% endcodeblock %}
修改仅自动获取IP,手动修改DNS---->广东地区114.114.114.114,202.96.128.86
{% codeblock lang:sh %}
乱码问题
	#VI乱码
	查看locale -a
	vi ~/.vimrc
		let &termencoding=&encoding
		set fileencodings=utf-8,gbk
	#GEANY乱码
	编辑->首选项->文件->编码->非缺省编码(已存在文件打开方式)	
	#rhythmbox 乱码	
	mid3iconv -e GBK *.mp3
	sudo gedit /etc/profile
	export PATH=$PATH GST_ID3_TAG_ENCODING=GBK:UTF-8:GB18030
	export PATH=$PATH GST_ID3V2_TAG_ENCODING=GBK:UTF-8:GB18030
{% endcodeblock %}
{% codeblock lang:sh %}
virtualbox
	net use x:\\vboxsvr\share
	mount -t vboxsf share mount_point
	sudo gpasswd -a [当前用户名] vboxusers
	#sudo apt-get install gnome-system-tools
	选择 用户和组－>编辑组－>添加当前用户
{% endcodeblock %}
{% codeblock lang:sh %}
开机挂载fstab
	查看UUID
	1.sudo blkid
	2.ls -l /dev/disk/by-uuid
	/dev/sda5: LABEL="softwareinstall" UUID="8A42CD9342CD8503" TYPE="ntfs" 
	vi /etc/fstab
	UUID=8A42CD9342CD8503   /media/channe/softwareinstall   ntfs    defaults        0       0  
	mount -a测试
{% endcodeblock %}
{% codeblock lang:sh %}
从ubuntu换到mint后,发现添加了许多开机启动脚本.如下:
/usr/lib/at-spi2-core/at-spi-bus-launcher --launch-immediately			AT-SPI D-Bus Bus
setxkbmap -option terminate:ctrl_alt_bksp								Ctrl Alt Backspace
dropbox start -i														Dropbox
fcitx-autostart
mate-keyring-daemon --start --components=gpg							GPG 密码助手
gsettings-data-convert													GSettings 数据转换
/usr/bin/mate-settings-daemon											MATE 设置守护程序
mintupdate-launcher	mintUpdate
/usr/lib/linuxmint/mintUpload/launch-file-uploader.py					mintUpload
mintwelcome-launcher													mintWelcome
/usr/lib/policykit-1-gnome/polkit-gnome-authentication-agent-1			PolicyKit Authentication Agent
/usr/lib/polkit-mate/polkit-mate-authentication-agent-1					PolicyKit 认证代理
start-pulseaudio-kde													PulseAudio Sound System KDE Routing Policy
start-pulseaudio-x11													PulseAudio 声音系统
/usr/bin/mate-keyring-daemon --start --components=ssh					SSH 密钥助手
xhost +																	Xhost +
/usr/bin/mate-keyring-daemon --start --components=secrets				保密存储服务
system-config-printer-applet											打印队列 Applet
mate-power-manager														电源管理器
mate-screensaver														屏幕保护程序
mate-bluetooth-applet													蓝牙管理器
nm-applet																管理网络连接
mate-volume-control-applet												音量控制
xdg-user-dirs-gtk-update  												用户文件夹更新 更新公共文件夹名称以匹配当前区域设置
/usr/bin/mate-keyring-daemon --start --components=pkcs11  				证书和密钥存储,MATE 密钥环：PKCS#11 组件
/usr/lib/vino/vino-server --sm-disable									GNOME 桌面共享服务器
而且可以选择让系统记住 注销时使用的程序
其实这些东西存放在/etc/xdg/autostart目录下.
{% endcodeblock %}