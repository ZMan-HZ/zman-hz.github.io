---
title: MacOS如何显示隐藏文件
date: 2020-08-19 20:26:53
comment: true
categories: OS技巧
tags:
- MacOS
- 隐藏文件
- Hidden File
---

MacOS上的隐藏文件都是以.(点)开头的文件夹或者文件，不像windows那样可以通过设置属性来使其隐藏。
<!--more-->
显示隐藏文件
``` bash
defaults write com.apple.finder AppleShowAllFiles -bool true;
KillAll Finder
```

不显示隐藏文件
``` bash
defaults write com.apple.finder AppleShowAllFiles -bool false;
KillAll Finder
```