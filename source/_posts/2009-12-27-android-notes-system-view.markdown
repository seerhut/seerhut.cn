---
author: admin
comments: true
date: 2009-12-27 07:29:10+00:00
layout: post
link: http://www.seerhut.cn/2009/12/27/android-notes-system-view/
slug: android-notes-system-view
title: Android Notes [system view]
wordpress_id: 220
---

android系统是一个典型的嵌入式linux，基础是linux kernel，整个android框架运行于一个称为dalvik的虚拟机之上，因此app是平台无关的，如果没有使用特殊的硬件，在armv6上运行的app可以直接放在android on mips中一致运行。事实上android为常用的硬件（gps,加速度感应等）提供了jni的接口，移植时只要重新把硬件操作包裹成.so供框架调用。

flash分为几个block：


<blockquote># cat /proc/mtd
dev:    size   erasesize  name
mtd0: 00040000 00020000 "misc"
mtd1: 00500000 00020000 "recovery"
mtd2: 00280000 00020000 "boot"
mtd3: 05a00000 00020000 "system"
mtd4: 05000000 00020000 "cache"
mtd5: 127c0000 00020000 "userdata"
mtd6: 20000000 00020000 "msm_nand"
#</blockquote>


boot存放kernel和ramdisk，dump出来后需要split_bootimg.pl或者unpack-H.pl来把两者分离出来。boot区的结构如下：


<blockquote>format (from bootimg.h)
** +-----------------+
** | boot header     | 1 page
** +-----------------+
** | kernel          | n pages
** +-----------------+
** | ramdisk         | m pages
** +-----------------+
** | second stage    | o pages
** +-----------------+
**
** n = (kernel_size + page_size - 1) / page_size
** m = (ramdisk_size + page_size - 1) / page_size
** o = (second_size + page_size - 1) / page_size</blockquote>


header中存放kernel和ramdisk的偏移信息：


<blockquote>header_format (from bootimg.h)
struct boot_img_hdr
{
unsigned char magic[BOOT_MAGIC_SIZE];

unsigned kernel_size;  /* size in bytes */
unsigned kernel_addr;  /* physical load addr */

unsigned ramdisk_size; /* size in bytes */
unsigned ramdisk_addr; /* physical load addr */

unsigned second_size;  /* size in bytes */
unsigned second_addr;  /* physical load addr */

unsigned tags_addr;    /* physical addr for kernel tags */
unsigned page_size;    /* flash page size we assume */
unsigned unused[2];    /* future expansion: should be 0 */

unsigned char name[BOOT_NAME_SIZE]; /* asciiz product name */

unsigned char cmdline[BOOT_ARGS_SIZE];

unsigned id[8]; /* timestamp / checksum / sha1 / etc */
};</blockquote>


这样系统就可以在启动时找到kernel和ramdisk。

ramdisk结构如下


<blockquote>$ tree root -p
root
|-- [drwxr-xr-x]  data
|-- [-rw-r--r--]  default.prop
|-- [drwxr-xr-x]  dev
|-- [-rwxr-xr-x]  init
|-- [-rw-r--r--]  init.goldfish.rc
|-- [-rw-r--r--]  init.rc
|-- [-rw-r--r--]  init.sapphire.rc
|-- [drwxr-xr-x]  proc
|-- [drwxr-xr-x]  sbin
|   `-- [-rwxr-xr-x]  adbd
|-- [drwxr-xr-x]  sys
`-- [drwxr-xr-x]  system</blockquote>


android不使用init.d管理启动过程，init读取init.xxx直接管理启动过程，主要完成重要目录、文件、环境变量的建立，挂载block，启动服务等。

system block存放系统目录，启动的时候ro挂载在ramdisk的/system下：


<blockquote>$ tree -p system -L 1
system
|-- [drwxr-xr-x]  app
|-- [drwxr-xr-x]  bin
|-- [-rw-r--r--]  build.prop
|-- [drwxr-xr-x]  etc
|-- [drwxr-xr-x]  fonts
|-- [drwxr-xr-x]  framework
|-- [drwxr-xr-x]  lib
|-- [drwxr-xr-x]  usr
`-- [drwxr-xr-x]  xbin</blockquote>


framework存放android框架文件，app存放系统自带的apps，以.apk的形式，不可由用户卸载。其他的目录沿袭linux传统。

userdata block存放用户数据，启动的时候rw挂载到/data，用户安装的apps存放于这里，apps的配置文件也在此目录中。

recovery block 存放一个小型的rootfs，recovery模式启动的时候会挂载此区域，此时系统一些基本的功能用于备份还原等管理功能。recovery模式下init过程较为简单，启动adbd服务后就执行recovery程序接受用户操作：


<blockquote>$ tree -p recovery -L 3
recovery
`-- [drwxr-xr-x]  root
|-- [drwxr-xr-x]  data
|-- [-rw-r--r--]  default.prop
|-- [drwxr-xr-x]  dev
|-- [drwxr-xr-x]  etc
|-- [-rwxr-xr-x]  init
|-- [-rw-r--r--]  init.goldfish.rc
|-- [-rw-r--r--]  init.rc
|-- [-rw-r--r--]  init.sapphire.rc
|-- [drwxr-xr-x]  proc
|-- [drwxr-xr-x]  res
|   |-- [drwxr-xr-x]  images
|   `-- [-rw-r--r--]  keys
|-- [drwxr-xr-x]  sbin
|   |-- [-rwxr-xr-x]  adbd
|   `-- [-rwxr-xr-x]  recovery
|-- [drwxr-xr-x]  sys
|-- [drwxr-xr-x]  system
`-- [drwxr-xr-x]  tmp</blockquote>
