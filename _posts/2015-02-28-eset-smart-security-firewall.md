title: eset smart security firewall
date: 2015-02-28 22:51:51
tags:
- firewall
- 杀软
categories: skill
description: ESS个人防火墙简单设置
feature: /images/masami/0001.jpg
toc: true
---
{% textcolor danger %}
适用下列情况.
{% endtextcolor %}
{% codeblock lang:sh %}
禁止特定程序连接wan网
但是又需要在局域网内传输数据
{% endcodeblock %}
优先级 允许 > 禁止.
举例: POWERDVD开启局域网dlna功能,但是防止激活失败(你懂的).
<!-- more -->
{% img http://7vztq1.com1.z0.glb.clouddn.com/2015/ESS防火墙简单设置/1.png [ESS [#1] %}
{% img http://7vztq1.com1.z0.glb.clouddn.com/2015/ESS防火墙简单设置/2.png [ESS允许部分设置 [#2] %}
{% img http://7vztq1.com1.z0.glb.clouddn.com/2015/ESS防火墙简单设置/4.png [ESS禁止部分设置 [#3] %}
{% textcolor danger %}
参考:
{% endtextcolor %}
```
http://www.aishadu.net/nod32-id
```