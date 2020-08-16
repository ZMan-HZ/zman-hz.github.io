---
title: MacOS 如何显示隐藏文件使用小技巧
categories: OS技巧
comments: true
date: 2020-08-16 19:13:24
tags:
- [MacOS]
- [隐藏文件]
---

MacOS上的隐藏文件都是以.开头的文件夹或者文件，不像windows那样可以通过设置属性来使其隐藏。
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