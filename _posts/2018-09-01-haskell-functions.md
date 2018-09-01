---
layout:     post
title:      Haskell Functions
subtitle:   
date:       2018-08-26
author:     EC
tags:
    - Haskel
---

### Haskell Functions

#####  `foldr` 的定义

```haskell
	foldr :: (a -> b -> b) -> b -> ([a] -> b)
	foldr f k [] = []
	foldr f k (x:xs) = f x (fold f k xs)
```

##### `foldr` 的应用

```haskell
	sum :: [Int] -> Int			product :: [Int] -> Int
	sum = foldr (+) 0			product	= foldr (×) 1
	
	and :: [Bool] -> Bool		or :: [Bool] -> Bool
	and = foldr (∧) True		or :: foldr (∨) False
	
	(++) :: [a] -> [a]			concat :: [a] -> [a]
	(++ ys) = fold (:) ys		concat [] = []
								concat 	
```

##### 其他函数



























### Reference

[A tutorial on the universality and expressiveness of fold](http://www.cs.nott.ac.uk/~pszgmh/fold.pdf)

