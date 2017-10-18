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

```
root@DD-WRT:/tmp# wget http://192.168.1.142/openwrt/ar71xx/openwrt-ar71xx-generic-tl-wr703n-v1-squashfs-factory.bin  
Connecting to 192.168.1.142 (192.168.1.142:80)  
openwrt-ar71xx-gener 100% |*******************************| 3840k 0:00:00 ETA 
```

Other methods can be found here :- http://wiki.openwrt.org/doc/howto/generic.sysupgrade

### Step 2 :

The TRICK is that the partition names are different between OpenWRT and DD-WRT.  Whereas all OpenWRT instructions will tell you to write to the ‘_firmware_‘ partition, this does not exist on DD-WRT and you have to use the ‘_linux_‘ partition instead.  Use the ‘mtd’ command as per the example below to write the OpenWRT image onto the router.  Note the ‘-r’ argument will reboot the router as soon as the flash is complete.  (As usual, do not power off or disconnect during the flashing!).

```
root@DD-WRT:/tmp# mtd -r write openwrt-ar71xx-generic-tl-wr703n-v1-squashfs-factory.bin linux  
Unlocking linux ...  
Writing from openwrt-ar71xx-generic-tl-wr703n-v1-squashfs-factory.bin to linux ... [e]  
Connection closed by foreign host.

```

## Install shadowsocks 

2、使用作者提供的软件源安装及更新SHADOWSOCKS
注：由于CC15.05正式版开始加入ipk包签名，使用下面的软件源会在opkg update时候遭遇签名验证失败（signature check failed），此时需要找到 /etc/opkg.conf 文件，将 option check_signature 1 这一行注释掉： #option check_signature 1 

软件源位置：http://openwrt-dist.sourceforge.net/packages/

由于该地址被部分ISP所墙，或者访问困难，所以这些ISP要以前面“1”的步骤为基础才能进行这一步。

打开Luci，定位到“系统”-“软件包”-“配置”选项卡，在软件源末尾加入两行：

- OpenWrt:

```
src/gz openwrt_dist http://openwrt-dist.sourceforge.net/packages/OpenWrt/base/ar71xx
src/gz openwrt_dist_luci http://openwrt-dist.sourceforge.net/packages/OpenWrt/luci

src/gz openwrt_dist http://openwrt-dist.sourceforge.net/packages/OpenWrt/base/ar71xx
src/gz openwrt_dist_luci http://openwrt-dist.sourceforge.net/packages/OpenWrt/luci

```

- LEDE:

```
src/gz openwrt_dist http://openwrt-dist.sourceforge.net/packages/LEDE/base/mipsel_24kc
src/gz openwrt_dist_luci http://openwrt-dist.sourceforge.net/packages/LEDE/luci

src/gz openwrt_dist http://openwrt-dist.sourceforge.net/packages/LEDE/base/mipsel_24kc
src/gz openwrt_dist_luci http://openwrt-dist.sourceforge.net/packages/LEDE/luci
```

请根据自己的CPU型号将ar71xx/mipsel_24kc替换成相应的文本，最后点击提交。然后执行命令 opkg update ，如果卡住，就按Ctrl+C取消掉，然后重试，如果反复重试还是不行，那么你可能还是需要自行下载ipk后手工安装、更新。

相关的依赖包： ip ipset iptables-mod-tproxy libev libudns libpcre libmbedtls libsodium ，首次安装shadowsocks的话要先把这些依赖包的最新版本安装上，如果是升级shadowsocks，如果看到这些依赖包有新版本也推荐更新一下。

接下来可以在过滤器中输入关键词进行搜索，分别输入shadowsocks、chinadns、dns-forwarder进行搜索，找到最新的shadowsocks-libev, luci-app-shadowsocks, chinadns, luci-app-chinadns, dns-forwarder, luci-app-dns-forwarder，分别点击安装即可；

顺带说明，此软件源同样包含ShadowVPN和Redsocks2及其luci-app，作者都是a65535，有需要的可以自行下载安装。
