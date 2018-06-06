title: 函数调用关系图
date: 2015-03-03 20:46:45
tags:
- pvtrace
- dot
categories: skill
description: 关于利用pvtrace和dot命令生成函数调用关系图
feature: /images/mainmenu/005.jpg
toc: true
---
经常看代码的人大多用过下面的这些工具.
*   [Source Navigator NG](http://sourceforge.net/projects/sourcenav/)
*   [Scientific Toolworks Understand](https://scitools.com/)
*   Source Insight

今天在windows平台下试验一下老早以前看过的"关于利用pvtrace和dot命令生成函数调用关系图".
生成关系大致是1->2->3->4.
<!-- more -->
### 1.c To exe ###
``` c
#include <stdio.h>
#include <stdlib.h>
void test1()
{
    printf("in test1.\n");
}
void test2()
{
    test1();
}
void test3()
{
    test1();
    test2();
}
int main(int argc, char *argv[])
{
    printf("Hello wolrd.\n");
    test1();
    test2();
    test3();
    return 0;
}
```
### 2.执行exe文件生成trace.txt ###
```
E0x004014B9
E0x004013FC
X0x004013FC
E0x00401436
E0x004013FC
X0x004013FC
X0x00401436
E0x00401475
E0x004013FC
X0x004013FC
E0x00401436
E0x004013FC
X0x004013FC
X0x00401436
X0x00401475
X0x004014B9
```
### 3.pvtrace命令利用"trace.txt" + "可执行exe文件"生成Dot文件 ###
```
digraph testexe {
  main [shape=rectangle]
  test1 [shape=ellipse]
  test2 [shape=rectangle]
  test3 [shape=rectangle]
  main -> test1 [label="1 calls" fontsize="10"]
  main -> test2 [label="1 calls" fontsize="10"]
  main -> test3 [label="1 calls" fontsize="10"]
  test2 -> test1 [label="2 calls" fontsize="10"]
  test3 -> test1 [label="1 calls" fontsize="10"]
  test3 -> test2 [label="1 calls" fontsize="10"]
}
```
### 4.dot命令利用dot文件生成PNG图片 ###
{% img http://7vztq1.com1.z0.glb.clouddn.com/2015/函数调用关系图/graph.png [函数调用关系图 [#1] %}
{% label Q1 danger %} 网上下载的pvtrace源码中包含instrument.c文件,其中几个函数解释了2_trace.txt文件的由来.
{% label Q2 danger %} windows平台下如果生成的trace.txt文件内容格式异常,自行更改pvtrace源码中instrument.c文件.
``` c
void __cyg_profile_func_enter( void *this, void *callsite )
{
  fprintf(fp, "E%p\n", (int *)this);
}
void __cyg_profile_func_exit( void *this, void *callsite )
{
  fprintf(fp, "X%p\n", (int *)this);
}
```
{% label Q3 danger %} 1中我的makefile.
``` makefile
#
# Makefile for test the trace utility.
#
#gcc -g -finstrument-functions -Wl,-Map,test.map test.c ./pvtrace/instrument.c -o test
#pvtrace test.exe
#dot -Tpng graph.dot -o graph.png
CC = gcc
CFLAGS = -g -finstrument-functions
OBJS = instrument.o test.o

test: $(OBJS)
	gcc -o $@ $(OBJS)

.c.o:
	$(CC) $(CFLAGS) -Wall -c $<

install: test
	cp test /usr/local/bin

clean:
	rm -f test *.o
```