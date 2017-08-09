---
author: admin
comments: true
date: 2008-11-14 06:55:22+00:00
layout: post
link: http://www.seerhut.cn/2008/11/14/%e5%85%b3%e4%ba%8estm32%e4%b8%adcan-filter%e7%9a%84%e7%ac%94%e8%ae%b0/
slug: '%e5%85%b3%e4%ba%8estm32%e4%b8%adcan-filter%e7%9a%84%e7%ac%94%e8%ae%b0'
title: 关于STM32中CAN Filter的笔记
wordpress_id: 97
---

Rx mailbox的identifier register有32位，最低三位是0:reserved  1:RTR 2:IDE，因此extID的最低位存于id register的低第3位；而stdID的最低位从id register的21位开始，stdID占据高11位。

filter bank register也是32位，和mailbox id register一一对应，因此filter bank bits对RTR和IDE也有效。STM32的fwlib中CAN_FilterIdHigh/Low CAN_FilterMaskIdHigh/Low都是raw bits，而RxMessage.StdId之类却由fwlib管理offset，因此两者不能直接对应，filter register需要手动移位来控制。

如果要接受stdID为0x*1*的数据帧：


<blockquote>/* CAN filter init */
CAN_FilterInitStructure.CAN_FilterNumber=0;
CAN_FilterInitStructure.CAN_FilterMode=CAN_FilterMode_IdMask;
CAN_FilterInitStructure.CAN_FilterScale=CAN_FilterScale_32bit;
CAN_FilterInitStructure.CAN_FilterIdHigh=0x10 << 5;
CAN_FilterInitStructure.CAN_FilterIdLow=0x0000 | 0x0 << 1 | 0x0 << 2;
CAN_FilterInitStructure.CAN_FilterMaskIdHigh=0xF0 << 5;
CAN_FilterInitStructure.CAN_FilterMaskIdLow=0x0000 | 0x1 << 1 | 0x1 << 2;
CAN_FilterInitStructure.CAN_FilterFIFOAssignment=CAN_FilterFIFO0;
CAN_FilterInitStructure.CAN_FilterActivation=ENABLE;
CAN_FilterInit(&CAN_FilterInitStructure);</blockquote>
