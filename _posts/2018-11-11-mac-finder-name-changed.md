---
layout:     post
title:      macOS 中 Finder 侧边栏的桌面变成 Desktop 的解决办法
subtitle:   Reset Localization for Desktop on Finder in OS X
date:       2018-11-11
author:     "EC"
header-img: "img/post-bg-mac.jpg"
catalog: true
comments: true
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
    - macOS
---

会变成中文名的文件夹下都有一个 `.localized` 的文件，如果不小心删掉了就会出现变成英文的情况，解决方法如下：

```bash
	cd desktop	
	touch .localized
	killall finder
```

