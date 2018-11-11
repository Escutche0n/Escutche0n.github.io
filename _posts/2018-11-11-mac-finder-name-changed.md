---
layout:     post
title:      OS X 中 Finder 侧边栏的桌面变成 Desktop 的解决办法
subtitle:   Localization reset for Desktop on finder sidebar in OS X
date:       2018-11-11
author:     "EC"
header-img: "img/post-abstract-2.jpg"
catalog: true
comments: true
tags:
    - mac
---

### OS X 中 Finder 侧边栏的桌面变成 Desktop 的解决办法

```bash
	cd desktop
	touch .localized
	killall finder
```

