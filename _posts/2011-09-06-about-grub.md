---
layout: post
title: "about grub and drivers for A"
date: 2011-09-06 01:42
comments: true
categories: skill
feature: /images/mainmenu/101.jpg
toc: true
---
### 1. drivers for ATI ###
1.1 更新内核或者显卡驱动造成重启后黑屏,需要<font color=red>卸载驱动</font>
{% codeblock lang:sh %}
sudo apt-get remove --purge fglrx fglrx_* fglrx-amdcccle* fglrx-dev*
{% endcodeblock %}
### 2. grub_rescue ###
仅限于分区文件未破坏(格式化)
{% codeblock lang:sh %}
sudo -i
fdisk -l
mount /dev/sdaX /mnt
mount /dev/sdaY /mnt/boot	#如果设置了单独的boot分区
grub-install --root-directory=/mnt/ /dev/sda
#或者set root='(hd0,msdosX)'
update-grub
grub-install /dev/sda
{% endcodeblock %}
<!-- more -->
### 3. os_install ###
dvd安装很简单,这里以livecd为例<br>
#### 3.1 ubuntu ####
{% raw %}
```
title install ubuntu 11.10
root (hd0,X)
kernel (hd0,X)/vmlinuz boot=casper iso-scan/filename=/ubuntu-11.10.iso ro quiet splash locale=zh_CN.UTF-8
initrd (hd0,X)/initrd.lz
boot
```
{% endraw %}
#### 3.2 fedora ####
{% raw %}
```
待续
```
{% endraw %}
#### 3.3 opensuse ####
{% raw %}
```
insmod part_msdos
linux (hd0,msdos7)/os_install/loader/linux kiwidebug=1
initrd (hd0,msdos7)/os_instal/loader/initrd
boot
#进入终端
mkdir -pv /mnt/tmp /livecd
mount /dev/sdaX /mnt/tmp
#注意iso所在分区sdaX,opensuse源生不识别ntfs
#特别注意安装过程是先分区再复制文件安装,不要把iso放在你想要安装的盘符上
mount -o loop /mnt/tmp/suse12.3.iso /cdrom
mount -o loop /mnt/tmp/suse12.3.iso /livecd
exit
```
{% endraw %}
### 4. 双多系统bug ###
从ubuntu系统不关机重启进入win系统,<font color=red>耳机输出无声音</font>.
有人说在退出ubuntu前勾选禁音选项,亲测无效!
WIN下睡眠可破!
### 5. win下小工具删除linux系统,无需重启 ###
* 使用DiskGenius或类似工具删除linux系统所在分区,也包括swap分区.
	PS.注意主扩展分区分布情况  (主+主+主+逻辑  主+主+主+主)
* xp使用		mbrfix /dirve 0 fixmbr /yes
* win7使用	mbrfix.exe /drive 0 fixmbr /win7 /yes
