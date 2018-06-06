title: "linux kernel bootargs&&update&&debug"
date: 2015-05-20 13:15:28
tags:
- kernel
- kdb
- bootargs
categories: skill
description: 对未来感到不安的时候,静静看书吧!
feature: /images/mainmenu/141.jpg
toc: true
---
#### kernel bootargs ####
做android机顶盒时候,有一个小小小需求:LK/BOOT阶段设置按键可以让android启动后进入adb模式.
这就涉及到lk向kernel传递bootargs以及usb驱动层获取args的问题.
以下是我的patch:
* 假设LK层获取了相应键值,处理程序加上
```
extern char g_CMDLINE [200];
sprintf(g_CMDLINE, "%s usb_mode=1", g_CMDLINE);
```
* kernel driver usb20.c
```
static int usb_mode;
static int __init boot_getusbmode_setup(char *str)
{
	get_option(&str, &usb_mode);
	printk("\n[k_test]%s:%d  usb_mode:%d\n", __FUNCTION__,__LINE__,usb_mode);
	return 1;
}
__setup("usb_mode=", boot_getusbmode_setup);
```
#### kernel update ####
/usr/src<!-- more -->
 ├── config-3.16.0-30-generic(cp from /boot)
 ├── config-3.16.0-37-generic(cp from /boot)
 ├── linux-4.0.4(untar from https://www.kernel.org/pub/linux/kernel//v4.x/linux-4.0.4.tar.gz)
 ├── linux-headers-3.16.0-30(origin)
 ├── linux-headers-3.16.0-30-generic
 ├── linux-headers-3.16.0-37(dist-upgrade)
 └── linux-headers-3.16.0-37-generic
 ```
 sudo apt-get install ncurses-dev
 make oldconfig/make menuconfig 
 make bzImage/make modules/make all
 make modules_install(/lib/modules/)
 make install(/boot/)
 cd /boot
 mkinitramfs -o /boot/initrd.img-4.0.4 /lib/modules/4.0.4
 update-grub2(/boot/grub/grub.cfg)
 ```
[参考](http://blog.chinaunix.net/uid-26000296-id-4208526.html)
#### kernel debug 来自互联网 ####
* menuconfig中配置调试选项,典型的调试选项位于“kernel hacking”中,很多有用的调试选项CONFIG_DEBUG_SLAB/CONFIG_KALLSYMS...,这种方法比较明显的缺陷：内核体积变大,需要重编内核.
* 打印,内核态用printk（用户态用printf）,这种方法明显的优点是：门槛低,上手快,定位问题有帮助；明显缺点：要不断的加打印和重编内核.同时,要注意优先级的问题.另外,有个技巧就是通过编译宏来控制打印是否使能及使用用户态接口还是内核接口适配你自己的打印宏,从而方便调试.最后,注意内核打印printk有优先级的问题,7种优先级,用户可以根据需要进行配置,也可以通过proc/sys/kernel/printk动态修改设置.
* 通过proc文件系统动态来确认及获取内核一些状态信息.同时,用户也可以实现自己的proc文件,来获取自己想要的信息.
* ioctl方法,其主要作用于文件描述符上,用户可以定制自己的命令及相应的动作机制,比较灵活.
* 通过strace来跟踪系统调用的参数、消耗时间及调用顺序等,比较有用的一个跟踪工具.
* oops信息分析.
* sysrq.这种方法主要针对系统假死的状态比较有效.使用前需要动态配置,使用ALT+SysRQ+各个功能组合.
* gdb调试内核,可以调试内核映像.
* kdb内核调试.
* kgdb调试,明显的缺点是需要两台机器,一台调试机,一台被调试机,通过串口调试.
* UML（User-Mode Linux）,简单的讲就是将linux作为一个用户进程进行调试,最大的缺点就是不能调试与硬件相关的功能,如驱动.
* LTT,内核的一个补丁包.
* Dprobe动态茶庄的一个工具.
* 指令断点kprobe,可以对内核的指令动态设置断点.
* 数据断点hw_breakpoint,可以对指令和数据都进行动态监控.
* perf调试,这个工具社区在不断地更新,而且集成了很多工具具有的功能,后面这个工具要引起重视.
* oprofile,其实这个工具主要对性能调试,采点关键的性能函数很有用.
* 内核热补丁,非常有用的利器,但可靠性是一个挑战性问题.
* 仿真器或模拟器,需要额外的硬件支持.