---
layout:     post
title:      "Flash DDWRT to Openwrt"
subtitle:   "提子帕奇"
date:       2017-10-18
author:     "l"
header-img: "img/post-bg-unix-linux.jpg"
tags:
    - Router
---


## USE THESE INSTRUCTIONS AT YOUR OWN RISK

I have only done this procedure once and sharing because of the lack of information on the web on how to do this.

Some of the sellers of the TL-WR703N are shipping the routers with DD-WRT pre-installed – some with a Chinese interface, some with an English interface.

Regardless, you need telnet or SSH access to the router as upgrading via the web interface will not work.

### Step 1 :

Get the OpenWRT image onto the router.  
e.g.

`root@DD-WRT:/tmp# wget http://192.168.1.142/openwrt/ar71xx/openwrt-ar71xx-generic-tl-wr703n-v1-squashfs-factory.bin  
Connecting to 192.168.1.142 (192.168.1.142:80)  
openwrt-ar71xx-gener 100% |*******************************| 3840k 0:00:00 ETA  `

Other methods can be found here :- http://wiki.openwrt.org/doc/howto/generic.sysupgrade

### Step 2 :

The TRICK is that the partition names are different between OpenWRT and DD-WRT.  Whereas all OpenWRT instructions will tell you to write to the ‘_firmware_‘ partition, this does not exist on DD-WRT and you have to use the ‘_linux_‘ partition instead.  Use the ‘mtd’ command as per the example below to write the OpenWRT image onto the router.  Note the ‘-r’ argument will reboot the router as soon as the flash is complete.  (As usual, do not power off or disconnect during the flashing!).

`root@DD-WRT:/tmp# mtd -r write openwrt-ar71xx-generic-tl-wr703n-v1-squashfs-factory.bin linux  
Unlocking linux ...  
Writing from openwrt-ar71xx-generic-tl-wr703n-v1-squashfs-factory.bin to linux ... [e]  
Connection closed by foreign host.`
