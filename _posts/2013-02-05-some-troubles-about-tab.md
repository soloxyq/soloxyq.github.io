---
layout: post
title: "一些软件中TAB键冲突"
date: 2013-02-05 23:41
tags: 
- table键
comments: true
categories: soft_use
feature: /images/mainmenu/yoona.jpg
toc: true
---
{% img http://i1299.photobucket.com/albums/ag63/bearxuqian/CHANNE/photobucket/taeyeon_runningman_zps1c1f1b59.gif %}
### OneNote 2013 ###
onenote中table默认是插入制表符,有时候不要表格,需要4空格位,很烦有没有?
<!--more-->
编程时,有时候我们也需要用4空格或者8空格来代替TABLE位,有没有?
onenote->文件->选项->自定义更正
{% img http://i1299.photobucket.com/albums/ag63/bearxuqian/CHANNE/photobucket/_zpsd60eaab1.png %}
{% img http://i1299.photobucket.com/albums/ag63/bearxuqian/CHANNE/photobucket/1_zps45583df8.png %}
根据需要设置就可以了.
以后只要在onenote中输入"/t"再按下空格就产生table(4/8空格)位了
### VIM ###
VIM自定义配置一般会设置自动补全,例如pri后加TAB键自动补全printf.
直接输入ctrl+i解决tab位的问题(ctrl+v ctrl+i试试)