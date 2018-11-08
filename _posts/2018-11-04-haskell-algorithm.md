---
layout:     post
title:      Haskell 常见算法排序
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

### Haskell 常见排序算法

##### Insert Sort 插入排序

```haskell
	insert :: Ord a => a -> [a] -> [a]
	insert x (y:ys)
		| x < y     = x : y : ys
		| otherwise = y : x : ys
		
	insertsort :: Ord a => [a] -> [a]
	insertsort [] = []
	insertsort (x:xs) = insert x (insertsort xs)
```



##### Bubble Sort 冒泡排序

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



##### Quick Sort 快速排序

```haskell
	quicksort :: Ord a => [a] -> [a]
	quicksort [] = []
	quicksort (x:xs) = quicksort ys ++ [x] ++ quicksort zs
		where ys = [y | y <- xs, y <= x]
			  zs = [z | z <- xs, z >  x]
```



##### Merge Sort 归并排序

```haskell
	mergesort :: Ord a => [a] -> [a]
	mergesort []  = []
	mergesort [x] = [x]
	mergesort xs  = merge (mergesort ys)(mergesort zs) where
		(ys,zs) = splitAt (length xs 'div' 2) xs

	merge [] ys = ys
    merge xs [] = xs
    merge (x:xs) (y:ys)
      | x <= y    = x : merge xs (y:ys)
      | otherwise = y : merge (x:xs) ys
```



<iframe width="560" height="315" src="https://escutche0n.github.io/counts" frameborder="0" allowfullscreen zoom="50%" ></iframe>