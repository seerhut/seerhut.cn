---
author: admin
comments: true
date: 2010-03-13 06:47:42+00:00
layout: post
link: http://www.seerhut.cn/2010/03/13/build-android-splash-screen/
slug: build-android-splash-screen
title: Build Android Splash Screen
wordpress_id: 221
---

累了，搞点轻松的玩意。

    
    convert -depth 8 splash.png rgb:splash.raw rgb2565 < splash.raw > splash.raw565


然后把splash.raw565烧入flash就好

    
    fastboot flash splash1 splash.raw565
