---
layout:     post
title:      Haskell 学习笔记 1
subtitle:   Haskell Note 1 - Data Type, Evaluation and Inhabitants
date:       2018-11-18
author:     "EC"
header-img: "img/post-abstract-1.jpg"
catalog: true
comments: false
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
    - Haskell
---

### 数据类型

### 惰性求职与及早求值

### 居民数量问题

**类型居留问题 (Type inhabitation)** 是如下问题：

给定一个类型 **τ**，是否存在一个 **`λ-`** 项 **`M`** 使得对于某个类型环境 **`Γ`** 有 **`Γ ⊢ M :  τ`** (其意味着 **`M`** 在上下文 `Γ` 中是 `τ` 的一项) ？如果回答是肯定的，则 **M** 被称为 **τ** 的 **居民 inhabitant**[^1]。

居民的数量 **The number inhabitants** 则是一个类型数据中拥有的所有居民的数量总和，其中 bottoms **`⊥`**[^2] 忽略不计。

如果 `data Unit = Unit` 指的是其有一个居民因为它只可能有一种值，那 `Bool` 就应该有两个 inhabitants，分别为 `True` 和 `False`。

以此类推，一个二元组 `(a, b)` 包含了一对 a 类数据 及 b 类数据，其中 a 类数据有 `|a|` 个居民，b 类数据有 `|b|` 个居民，其两两相合会有 `|a| * |b|` 种组合，即 `(a, b)` 的居民数量为 `|a| * |b|`。

如果是 `Either` 的话二者选其一，则是两者之和 `|a| + |b|`。

而空函数 Void 根据其定义 `data Void` 没有其居民，可是它在 Haskell 中不是一个有效的数据类型。

----
[^1]: https://en.wikipedia.org/wiki/Type_inhabitation
[^2]: 未定义的值 即 undefined value

