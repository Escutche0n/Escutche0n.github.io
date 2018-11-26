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

### 居民数量

**类型居留问题 (Type inhabitation)** 是如下问题: 给定一个类型 **τ**，是否存在一个 **λ-** 项 **M** 使得对于某个类型环境 **Γ** 有 **Γ ⊢ M :  τ** ？如果回答是肯定的，则 **M** 被称为 **τ** 的 **居所 inhabitant**[^1]。

[^1]: https://en.wikipedia.org/wiki/Type_inhabitation