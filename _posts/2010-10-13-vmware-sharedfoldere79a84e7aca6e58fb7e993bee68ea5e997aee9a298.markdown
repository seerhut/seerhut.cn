---
author: admin
comments: true
date: 2010-10-13 13:31:58+00:00
layout: post
link: http://www.seerhut.cn/2010/10/13/vmware-sharedfolder%e7%9a%84%e7%ac%a6%e5%8f%b7%e9%93%be%e6%8e%a5%e9%97%ae%e9%a2%98/
slug: vmware-sharedfolder%e7%9a%84%e7%ac%a6%e5%8f%b7%e9%93%be%e6%8e%a5%e9%97%ae%e9%a2%98
title: vmware sharedFolder的符号链接问题
wordpress_id: 272
---

win guest无法识别share Folder中的符号链接，解决办法：

在虚拟机的vmx文件中添加

sharedFolder0.followSymlinks = "TRUE"
这问题很久前就碰到了，怎么就一直没仔细搜一下呢
