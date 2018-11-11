---
layout:     post
title:      OS X 中 Finder 侧边栏的桌面变成 Desktop 的解决办法
subtitle:   Reset Localization for Desktop on Finder in OS X
date:       2018-11-11
author:     "EC"
header-img: "img/post-bg-mac.jpg"
catalog: true
comments: true
tags:
    - OS X
---

会变成中文名的文件夹下都有一个.localized的文件，如果你不小心删掉了就会出现变成英文的情况，解决方法如下：

```bash
	cd desktop	
	touch .localized
	killall finder
```