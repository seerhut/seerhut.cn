---
author: admin
comments: true
date: 2008-03-19 06:17:42+00:00
layout: post
link: http://www.seerhut.cn/2008/03/19/%e5%85%b3%e4%ba%8earm%e5%b9%b3%e5%8f%b0%e4%b8%8a%e5%bb%ba%e7%ab%8blinux-rootfs%e7%9a%84%e5%90%8e%e8%af%9d/
slug: '%e5%85%b3%e4%ba%8earm%e5%b9%b3%e5%8f%b0%e4%b8%8a%e5%bb%ba%e7%ab%8blinux-rootfs%e7%9a%84%e5%90%8e%e8%af%9d'
title: 关于ARM平台上建立linux rootfs的后话
wordpress_id: 70
---

只有从源码建立才能最大限度保证兼容和稳定，尤其是现在各个发行套件使用的glibc甚至ABI版本都不同，因此必须使用一个buildroot套件进行工作。我选的是OpenEmbeded ( [http://www.openembedded.org](http://www.openembedded.org/)  )，使用12/2007发布的stable版本，库和程序较新，又很稳定，可以轻松建立从bootstrap到desktop的环境，同时保持很不错的可定制能力，最难得的是这个套件生成的rootfs几乎是最省空间的。

为了开发方便，可以把rootfs根目录直接挂上工作站的某个 NFS path，这样对文件系统的修改就方便很多。NFS的参数要作为args传给内核，内核中必须打开IP自动配置和NFS支持。挂载上rootfs开始init后系统会remount一次，这回用的不是kernel args里面指定的IP配置了，因此要修改启动脚本让网络环境设置正确。我在这里卡了一点时间，由于dhcp程序不能正常工作，将之nuke掉就好了。

一开始rootfs不用太精简，略微臃肿一些无妨，反正是用NFS rootfs。这样可以避免缺少某些包而启动失败，等系统运行正常再开始剪裁也不迟。注意OE的一些bootstrap-image helloworld-image等等精简系统似乎不能适应NFS rootfs环境，我在这里转了很大一圈，现在确定可以使用的最简配置如下：
<!-- more -->
` ANGSTROM_EXTRA_INSTALL ?= ""
#DISTRO_SSH_DAEMON ?= "dropbear"
IMAGE_INSTALL =" \
base-files \
base-passwd \
busybox \
mdev \
cron \
initscripts \
#modutils-initscripts \
netbase \
update-alternatives \
sysvinit \
sysvinit-pidof \
tinylogin \
angstrom-version \
#ppp \
setserial \
#nfs-utils \
#portmap \
mtd-utils"`
`
export IMAGE_BASENAME = "seerhut-test-image"
IMAGE_LINGUAS = ""
inherit image
`

同时/bin/login的owner必须是root。设置成u+s也不行。这样基本就完成了。

商业发布的套件几乎全是生成old arm abi二进制文件 的，而且同时发布的rootfs都很臃肿，不便剪裁。如果一定要使用商业套件，推荐timesys的toolchain，但是它的rootfs太大了，<32M的NAND几乎无法使用。
