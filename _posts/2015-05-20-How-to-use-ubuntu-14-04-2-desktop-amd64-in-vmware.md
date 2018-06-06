title: "use ubuntu-14.04.2-desktop-amd64 in vm11"
date: 2015-05-20 11:59:04
tags:
- shell
- ubuntu
categories: skill
description: 对未来感到不安的时候,静静看书吧!
feature: /images/mainmenu/042.jpg
toc: true
---
#### *sudo passwd root ####
#### *remove or purge无关软件 ####
```
ibus-* libreoffice-* thunderbird-*
sudo apt-get autoremove
```
#### *更改home目录文件夹中英文名称 ####
```
export LANG=en_US
xdg-user-dirs-gtk-update
export LANG=zh_CN
```
#### *dist-upgrade ####
#### *install some ####
```
sudo apt-get install dkms linux-headers-$(uname -r) build-essential psmisc ssh system-config-samba git ctags python-qt4
```
#### *VMware-Tools ####
install vmware-tools时出现关于vmhgfs.o的错误.<!-- more -->
在[stackoverflow](http://stackoverflow.com/questions/28579182/error-installing-vmware-tools-9-9-after-upgrade-to-linux-image-3-13-0-46/28579368#28579368)外国人讨论中找到[patch inode.c](https://gist.github.com/battlecow/cb3ea9f4bafc1391d708)
{% codeblock lang:sh %}
cd vmware-tools-distrib
cd lib/modules/source
tar xvf vmhgfs.tar
cd vmhgfs-only
vi inode.c(USE patch inode.c)
tar cvf vmhgfs.tar vmhgfs-only
cd vmware-tools-distrib
sudo ./vmware-install.pl
{% endcodeblock %}
#### *config some ####
multi-soft config like [This](https://gitcafe.com/channe/Ubuntu_Home)
{% textcolor danger %}
eg. vim config...
{% endtextcolor %}