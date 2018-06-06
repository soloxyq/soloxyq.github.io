title: "Route_Ai-BR100"
date: 2014-10-15 00:39:46
tags:
- Ai-BR100
categories: skill
description: 小度路由器刷机
feature: /images/mainmenu/20140515202338_BtUun.jpeg
toc: true
---
## reminiscence ##
大学时看过一篇blog,讲说PC软硬件厂商互相推动,"狼狈为奸",没有需求硬是创造需求,但是商业市场是支持的,
因为通过竞争,抢夺市场,科技更快的向前发展,技术水平不断进步,这是符合人类社会发展方向的,是客观的.
根据这场竞争中巨头推出产品的速度,总结出了"摩尔定律".
<!-- more -->
## grumble ##
今天看来,随着PC出货量每况愈下,这一现象已经转移到了其他市场.
对我这种"够用就好"的人来说,是很讨厌这种现象的.电脑还能三四年换一换,手机一年不换就没人关心了.
对我这种无聊刷刷机的人来说,是无法忍受的.
像华为,百度这种"大公司",推出的手机或者路由器等个人产品,软件支持时间大概是一年,甚至更短.
买个新手机,刚开始十天半个月推送个更新包,修修BUG,过个半年基本无人问津了.
可气的是官方不会说停止支持,像"起点"某些写小说太监的一样,有的是真的太监了,有的过个半年突然起死回生更新
了一两章.这造就了无数的民间大神,官方不更新让我来.所以有幸,只要多搜索,刷机党不会无事可"作"的.
{% textcolor danger %}PS:我要吐槽小度路由,顶着"三贱"的名号出世,其生命周期实在是短.{% endtextcolor %}
## 记录下小度路由刷机方法 ##
### record1 ###
``` bash
[点我](http://downloads.openwrt.org.cn/PandoraBox/Baidu-BR100)
下载2个bin文件,上面网址中根目录下是可以理解为OS的东西(小于8M), uboot目录里是可以理解为GRUB2的东西(大约128K).
路由lan口有线连接电脑,设置同一网段,电脑上开户一个tftp服务的目录.把下载的uboot.bin文件放进去.
telnet登录192.168.8.1(我使用的是putty工具,账号admin,密码空)
cd tmp
tftp -g -l u-boot.bin -r u-boot.bin 192.168.8.103
mtd_write write u-boot.bin /dev/mtd0(write命令执行前ls确认下是否成功下载bin文件)
reboot
```
### record2 ###
uboot刷成功后,我们要进入恢复模式(recovery,刷过android的会很熟悉)
``` bash
按住Reset按键，接入电源，等待WLAN灯闪烁
本地IP设置在192.168.1.*网段,访问[http://192.168.1.1/index.html](http://192.168.1.1/index.html)
选择固件->上传->更新
```
### record3 ###
下面是我的小度路由的信息
``` bash
CPU	MediaTek MT7620 E2 ver 2, eco 3
内存	64MB DDR2
闪存	Macronix MX25L6435E @ 25MHz (8MB)
以太网	IC Plus IP175C (built-in)
系统频率	CPU: 600MHz, Bus: 200MHz, Ref: 20MHz
编译日期	2014-10-12 23:50:19 +08:00
MAC地址		**:27:1E:6B:F5:**
```