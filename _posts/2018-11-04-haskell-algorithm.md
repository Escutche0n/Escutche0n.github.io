---
layout:     post
title:      常见算法排序 Haskell
subtitle:   Alogrithms in Haskell
date:       2018-11-04
author:     "EC"
header-img: "img/post-abstract-3.jpg"
catalog: true
comments: true
tags:
    - Haskell
    - 算法
---

### 常见排序算法 - Haskell



##### Bubble Sort

```haskell
	bubblesort :: Ord a => [a] -> [a]
	bubblesort xs
		| ys == xs  = ys
		| otherwise = bubblesort ys
		where ys    = bubble xs
	
	bubble :: Ord a => [a] -> [a]
	bubble (x: y: zs)
		| x > y		= y : bubble (x:zs)
		| otherwise = x : bubble (y:zs)
	bubble xs		= xs
```



##### Quick Sort

```haskell
	quicksort :: Ord a => [a] -> [a]
	quicksort [] = []
	quicksort (x:xs) = quicksort ys ++ [x] ++ quicksort zs
		where ys = [y | y <- xs, y <= x]
			  zs = [z | z <- xs, z >  x]
```



##### Merge Sort

```haskell
	mergesort :: Ord a => [a] -> [a]
	mergesort []  = []
	mergesort [x] = [x]
	mergesort xs  = merge (mergesort ys)(mergesort zs) where
		(ys,zs) = splitAt (length xs 'div' 2) xs

	merge []     ys     = ys
    merge xs     []     = xs
    merge (x:xs) (y:ys)
      | x <= y     = x : merge xs (y:ys)
      | otherwise  = y : merge (x:xs) ys
```



