---
layout:     post
title:      Worksheet 答案
subtitle:   Answer
date:       2018-11-18
author:     "EC"
header-img: "img/post-abstract-2.jpg"
catalog: false
comments: true
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
    - Haskell
---

#### Worksheet 3

###### Pattern Matching

1. Using the pattern matching and recursive functions:

   **(d)** Define a function `merge :: [Int] -> [Int] -> [Int]` where `merge xs ys` takes two lists `xs` and `ys` that are sorted in ascending order, and produces a list containing all their elements in ascending order.

   ```haskell
   	merge :: [Int] -> [Int] -> [Int]
   		merge xs [] = xs		-- pattern match base case
   		merge [] ys = ys		-- pattern match base case
   		merge (x:xs) (y:ys)
   			| x <= y 	= x : merge xs (y:ys)
   			| otherwise = y : merge (x:xs) ys
   ```

###### List Comprehension

我周末整理了 [List Comprehension](http://chenyuefu.icu/2018/11/18/haskell-list-comprehension/)，笔记在这儿，打算之后写更多不懂的知识点。 

1. Using list comprehensions:

   **(d)** Define the function `cartesian :: [a] -> [b] -> [(a, b)]` which returns the cartesian product of two infinite lists.

   ```haskell
   	cartesian :: [a] -> [b] -> [(a, b)]
   	cartesian [x] [y] = [(x, y)| x <- Num a, y <- Num a]
   ```

   ```haskell
   	cartesian :: [a] -> [b] -> [(a, b)]
   	cartesian xs ys = [(x, y) | x <- xs, y <- ys]
   ```

###### Infinite Lists

1. Given the infinite lists `xs :: [a]` and `ys :: [a]`, define the function `interleave :: [a] -> [a] -> [a]` which eventually outputs all the elements in `xs` and `ys`.

   ```haskell
   	interleave :: [a] -> [a] -> [a]
   	interleave [] ys = ys
   	interleave (x:xs) ys = x : interleave ys xs
   	-- 两个列表分别挨着抓第一个数出来组成新的列表
   ```

------

#### Worksheet 4

###### Folding

3. Use `foldr` to give definitions of the following functions, an for the remainder of this sheet.

   **(c)** Define a function called `and :: [Bool] -> Bool` that returns `True` when all the elements of the input list are `True` and returns `False` otherwise.

   ```haskell
   	and :: [Bool] -> Bool
   	and :: foldr (`and`) True		-- 所有条件均成立 and True 才可以
   ```

   ```haskell
   	and :: [Bool] -> Bool
   	and :: [] = False
   	and :: (x:xs)
   		| x == True = and xs
   		| otherwise = False
   ```

   **(e)** Define a function called `all :: (a -> Bool) -> [a] -> Bool` where the result of all `p xs` is `True` if for all elements `x` in `xs`, `p x` is `True`. Otherwise, this should return a `False`.

   ```haskell
   	all :: (a -> Bool) -> [a] -> Bool
   	all p [] =[]
   	all p (x:xs)
   		| p x = x: all p xs
   		| otherwise = all p xs	
   		-- 帮我问一下 TA 这个对不对
   		-- [🤦‍我感觉是错的]
   ```

   ```haskell
   	all :: (a -> Bool) -> [a] -> Bool
   	all = foldr (p . `and`) True
   ```

-----

#### Worksheet 5

###### Type Classes

1. The `pretty` type class is defined as follows:

   ```haskell
   	class Pretty a where
   		pretty :: a -> String
   ```

   **(a)** Define the `Pretty` instance for this data structure. Be creative!

   ```haskell
   	instance Pretty Reaction where
           Pretty Happy 		= ":)"
           Pretty Sad			= ":("
           Pretty Excited		= ":X"
           Pretty Angry		= ":*R#$"
           Pretty Indifferent	= ": X("
   ```

   ```haskell
   	-- 定义 instance 类型
   	-- instance 
   ```

###### Monoids

1. 

   **(c)** Write a function `crush :: Monoid m => [m] -> m` which crushes a list containing monoidal structure into a value of that monoid whilst preserving the monoid laws for the underlying monoid. For exmaple, `crush [1, 2, 3, 4] = 1 + 2 + 3 + 4` under the monoid `<m, (+), 0>`.

   ```haskell
   	crush :: Monoid m => [m] -> m
   	crush = foldr mappend mempty
   ```

---

### Worksheet 6

###### Tree

1. Here are some variations of tree structures:

   ```haskell
   	data Tree a = Leaf a | Fork (Tree a) (Tree a)
   	data Bush a = Tip | Node (Bush a) a (Bush a)
   	data Tush a = TLeaf a | TNode (Tush a) a (Tush a)
   	data RoseTree a = RoseLeaf a | RoseFork [RoseTree a]
   	data Rose Bush a = Rose Bush a [Rose Bush a]
   ```

   **(a)** Draw an example of each of these trees, and write values that correspond to your drawing.

   **(b)** Which of these can be instantiated without any value of type `a`?

   > *Bush and Rose Bush can be instantiated without any value of a inside.*