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

# 优化

```
# max open files
fs.file-max = 1024000
# max read buffer
net.core.rmem_max = 67108864
# max write buffer
net.core.wmem_max = 67108864
# default read buffer
net.core.rmem_default = 65536
# default write buffer
net.core.wmem_default = 65536
# max processor input queue
net.core.netdev_max_backlog = 4096
# max backlog
net.core.somaxconn = 4096

# resist SYN flood attacks
net.ipv4.tcp_syncookies = 1
# reuse timewait sockets when safe
net.ipv4.tcp_tw_reuse = 1
# turn off fast timewait sockets recycling
net.ipv4.tcp_tw_recycle = 0
# short FIN timeout
net.ipv4.tcp_fin_timeout = 30
# short keepalive time
net.ipv4.tcp_keepalive_time = 1200
# outbound port range
net.ipv4.ip_local_port_range = 10000 65000
# max SYN backlog
net.ipv4.tcp_max_syn_backlog = 4096
# max timewait sockets held by system simultaneously
net.ipv4.tcp_max_tw_buckets = 5000
# TCP receive buffer
net.ipv4.tcp_rmem = 4096 87380 67108864
# TCP write buffer
net.ipv4.tcp_wmem = 4096 65536 67108864
# turn on path MTU discovery
net.ipv4.tcp_mtu_probing = 1


# forward ipv4
net.ipv4.ip_forward = 1

```

## 保存生效

```
sysctl -p

```
