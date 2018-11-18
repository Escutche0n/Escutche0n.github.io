---
layout:     post
title:      List Comprehension
subtitle:   大概是叫列表解析式
date:       2018-11-18
author:     "EC"
header-img: "img/post-abstract-1.jpg"
catalog: true
comments: true
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
    - Haskell
    - 笔记
---

List Comprehension 来源于数学中的集合表达式：

$a_{1}$

它的作用是，从 **既有** 的 **集合** 之中按照规则产生新的 **集合**。

例如从无限长列表中提取前十个偶数：

```haskell
	prompt > [x * 2 | x <- [1..10]]
	[2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
```

其中 `|` 左端的为输出函数，`x` 是变量， `<-` 之后的是变量的范围。

```
	[关于 x 的输出函数 | x <- (x 的范围), 限制条件]
```

例如用自然数 `nats :: [Integer]` 来表示 `squares` 输出列表中所有自然数的平方：

```haskell
	squares :: [Integer]
	squares = [x * x | x <- nats]
```

定义名为笛卡尔的函数 `cartesian :: [a] -> [b] -> [(a, b)]`， 其返回值为其两个列表的笛卡尔数对。

```haskell
	cartesian :: [a] -> [b] -> [(a, b)]
	cartesian xs ys = [(x, y) | x <- xs, y <- ys]
```

我们