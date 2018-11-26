---
layout:     post
title:      Haskell 学习笔记 1
subtitle:   Haskell Note 1 - Data Type, Evaluation and Inhabitants
date:       2018-11-26
author:     "EC"
header-img: "img/post-abstract-1.jpg"
catalog: true
comments: false
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
    - Haskell
---

### 数据类型

一个数据尤其原始的数据类型，PoD。

### 惰性求职与及早求值

Eager Evaluation & Lazy Evaluation

一个是从外到里 一个是从里到外，两者有其一则停止。

当某一级无法继续推进的时候则先算下一级。

### 居民数量问题 The Number Inhabitants

根据 *Wikipedia*[^1]，**类型居留问题 (Type inhabitation)** 是这样的：

> 给定一个类型 **`τ`**，是否存在一个 **`λ-`** 项 **`M`** 使得对于某个类型环境 **`Γ`** 有 **`Γ ⊢ M :  τ`** (其意味着 **`M`** 在上下文 `Γ` 中是 **`τ`** 的一项) ？如果回答是肯定的，则 **M** 被称为 **τ** 的 **居民 inhabitant**。

简单的来讲，一个类型的 **inhabitant** 就是一个这个类型的**值**，例如 `\x -> x+1` (对应数学的 `f (x) = x + 1` )就是 `Int -> Int` 类型中的一个 **inhabitant**。学霸口中的 "type inhabitants" 和学渣说的 "values of that type" 大概是一回事，下文中我均以 ***居民*** 代替 ***inhabitant*** (s) 。

**居民的数量** 则是一个类型数据中 **拥有的所有居民的数量总和**，其中 bottoms **`⊥`**[^2] 忽略不计。

如果 `data Unit = Unit` 指的是其有一个居民因为它只可能有一种值，那 `Bool` 就应该有两个居民，分别为 `True` 和 `False`。

以此类推，一个二元组 `(a, b)` 包含了一对 `a` 类数据 及 `b` 类数据，其中 `a` 类数据有 `|a|` 个居民，`b` 类数据有 `|b|` 个居民，其两者相乘会有 `|a| * |b|` 种组合，即 `(a, b)` 的居民数量为 `|a| * |b|`。

如果是 `Either` 的话二者选其一，则是两者相加即`|a| + |b|` 种组合。

而空函数 `Void` 根据其定义 `data Void` 没有居民，值得注意的是它在 Haskell 中 **并非** 一个<u>有效</u>的数据类型。



https://gist.github.com/pchiusano/444de1f222f1ceb09596



----
[^1]: 摘自 Wikipedia: Type Inhabitation https://en.wikipedia.org/wiki/Type_inhabitation 和其中文版本。
[^2]: 未定义的值 即 undefined value。

