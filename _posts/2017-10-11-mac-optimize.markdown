---
layout:     post
title:      "Mac Service Optimze"
subtitle:   "飞起"
date:       2017-10-11
author:     "fladeer"
header-img: "img/post-bg-js-module.jpg"
tags:
    - Mac
---


- Disable Spotlight
```
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist
```


- Disable Bonjour

```
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.mDNSResponderHelper.plist
```




- Disable Apple Push

```
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.apsd.plist
```


- Kill Dock

```
defaults write com.apple.dashboard mcx-disabled -boolean YES
killall Dock
```

- Kill Notification Center

```
launchctl unload -w /System/Library/LaunchAgents/com.apple.notificationcenterui.plist
killall NotificationCenter
```


- Disable VM

```
sudo pmset -a hibernatemode 0
```
