---
author: admin
comments: true
date: 2008-05-20 09:17:50+00:00
layout: post
link: http://www.seerhut.cn/2008/05/20/usb%e8%bd%ac%e4%b8%b2%e5%8f%a3%e7%ba%bf%e4%bd%bf%e7%94%a8%e8%ae%b0%e5%bd%95%e5%8f%8a%e5%85%b6%e4%bb%96/
slug: usb%e8%bd%ac%e4%b8%b2%e5%8f%a3%e7%ba%bf%e4%bd%bf%e7%94%a8%e8%ae%b0%e5%bd%95%e5%8f%8a%e5%85%b6%e4%bb%96
title: usb转串口线使用记录及其他
wordpress_id: 79
---

把vxworks usart 的波特率搞错了，结果这些天像没头苍蝇一样。不知为什么总是固执的以为9600，结果今天回去看了眼代码发现是38400，忏悔中。。。。。
这样的话当时用linux开发板连vxworks再用ssh登上linux开minicom也是可行的了，不过既然买了usb转串口线就不折腾板子了。
下面是正题，买了两条usb串口线，一条50－60RMB，Z-TEK的;一条BAFO,110买的。两条都可以连linux，但是只有BAFO连vxworks正常，z-tek乱码。卖线的人说BAFO可以连cisco的路由，开来不是吹牛，在此提醒诸位vxworks的生徒要选BAFO的线。。。
