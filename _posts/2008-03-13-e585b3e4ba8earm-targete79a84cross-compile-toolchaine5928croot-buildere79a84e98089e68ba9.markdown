---
author: admin
comments: true
date: 2008-03-13 15:53:09+00:00
layout: post
link: http://www.seerhut.cn/2008/03/13/%e5%85%b3%e4%ba%8earm-target%e7%9a%84cross-compile-toolchain%e5%92%8croot-builder%e7%9a%84%e9%80%89%e6%8b%a9/
slug: '%e5%85%b3%e4%ba%8earm-target%e7%9a%84cross-compile-toolchain%e5%92%8croot-builder%e7%9a%84%e9%80%89%e6%8b%a9'
title: 关于ARM target的cross compile toolchain和root builder的选择
wordpress_id: 69
---

当然是在x86 host上的。
首先试了uclibc的buildroot，可编译toolchain和rootfs，svn更新。使用很方便，用了类似内核和busybox的配置界面menuconfig，可定制项目较细，自动化程度好。但是有个硬伤，不能选择使用glibc，强制使用uclibc。。。。但可能有patch能去掉这个限制。考虑到这个项目的主页也是buildroot.uclibc.org就不难理解这个限制了。对于使用uclinux的用户倒是很合适。

[http://buildroot.uclibc.org](http://buildroot.uclibc.org)

其次是timesys的商业套件，直接提供成品toolchain和BSP。大致每个季度更新，可以下载个人版，基本的工具还是全的。这公司似乎同atmel和arm联系较紧密 ，技术应该很可靠，但是要真正做项目个人版还是不够的，不过个人版提供的一些资料还是很由用处。顺便说一句，我的ebd9261就是提供了这个作为开发工具。

[http://www.timesys.com](http://www.timesys.com)

接着是open embedded，可编译toolchain和target package。总得来说做得不错，自动化程度很高，随时更新（用户从版本库检出更新），使用配置文件进行基本配置，可以选择任意包编译。build package的时候会把需安装的文件单独存放，而且粒度很细，比如vim就分放在vim vim-common vim-doc vim-dev vim-syntax vim-dbg vim-help等目录下，同时也提供存放完整vim文件的目录，还提供上述所有的ipk包，总之很是贴心。但是本项目使用的版本管理工具略显怪异。。

[http://www.openembedded.org](http://www.openembedded.org)

随后是gentoo自己的crossdev，这个没什么可说的，四海一家的方案。。。

[http://www.gentoo.org/proj/en/base/embedded/handbook](http://www.gentoo.org/proj/en/base/embedded/handbook)
