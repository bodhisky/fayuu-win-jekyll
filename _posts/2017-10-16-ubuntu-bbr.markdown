---
layout:     post
title:      "Ubuntu开启BBR"
subtitle:   "GComputeEngine"
date:       2017-10-16
author:     "Hux"
header-img: "img/post-bg-unix-linux.jpg"
tags:
    - 提子
---


## 开启bbr

开机后 `uname -r` 看看是不是内核 >= 4.9

执行 `lsmod | grep bbr`，如果结果中没有 `tcp_bbr` 的话就先执行

```
modprobe tcp_bbr
echo "tcp_bbr" >> /etc/modules-load.d/modules.conf
```

执行

```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
```

保存生效  
`sysctl -p`

执行

```
sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control
```

如果结果都有 `bbr`, 则证明你的内核已开启 bbr

执行 `lsmod | grep bbr`, 看到有 tcp_bbr 模块即说明 bbr 已启动
