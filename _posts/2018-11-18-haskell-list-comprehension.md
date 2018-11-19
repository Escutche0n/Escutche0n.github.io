---
layout:     post
title:      Haskell 列表解析式
subtitle:   List Comprehension in Haskell
date:       2018-11-18
author:     "EC"
header-img: "img/post-abstract-1.jpg"
catalog: false
comments: true
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
    - Haskell
---

它的作用是，从 **已知** 的 **集合** 之中 **按照规则** 产生新的 **集合**。

例如从无限长列表中提取前十个偶数：

```haskell
	ghci > [x * 2 | x <- [1..10]]
	[2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
```

其中 `|` 左端的为输出函数，`x` 是 **变量**， `<-` 之后的是**变量的范围**：

```
	[关于 x 的输出函数 | x <- (x 的范围), 限制条件]
```



例如用自然数 `nats :: [Integer]` 来表示 `squares` 输出列表中所有自然数的平方：

```haskell
	squares :: [Integer]
	squares = [x * x | x <- nats]
```

我们也可以在其基础上添加更多的限制条件，例如 ’7的倍数‘ 的平方：

```haskell
	multiSevenSquares :: [Integer]
	multiSevenSquares = [x * x | x <- nats, x `mod` 7 == 0]
```

---

除此之外，我们也可以用 list comprehension 来表示 `map` 和 `filter` 等函数：

`map` 函数将一组 `a` 类数据转化为一组 `b` 类数据：

```haskell
	map :: (a -> b) -> [a] -> [b]
	map f xs = [f x | x <- xs]
```

`filter` 函数将一组函数里满足条件 `p` 的元素过滤成新的列表：

```haskell
	filter :: (a -> Bool) -> [a] -> [a]
	filter p x = [x | x <- xs, p x]
					-- x 属于 xs 为变量范围 而 p x (== True) 为限制条件
```

定义名为笛卡尔的函数 `cartesian :: [a] -> [b] -> [(a, b)]`， 其返回值为其两个原始列表的笛卡尔数对：

```haskell
	cartesian :: [a] -> [b] -> [(a, b)]
	cartesian xs ys = [(x, y) | x <- xs, y <- ys]
```

最后我们也可以用它来求一个列表的长度重新定义`length'`函数：

```haskell
	length' = sum[1| _ <- xs] 	-- 计算 xs 转化为单个列表元素的数量即为列表的长度
```

