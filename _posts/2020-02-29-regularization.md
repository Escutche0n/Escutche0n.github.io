---
layout:     post
title:      ARM Compiler Installation for Daniel Page's Lab
subtitle:   "史上最简单易懂、全面详细的“正则化”教程"
date:       2020-02-29
author:     "LoveMIss-Y"
catalog: true
comments: true
header-img: "img/post-bg-arm.jpg"
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
  - Python
  - Data Science
  - CSDN
---



### 模型过拟合

训练的模型过拟合，根据方差+偏差的分解，则说明“方差”很大，直观的含义就是，模型的稳定性不强，表现在某一个特征输入数据“稍有波动”，模型的效果会变差。因为在测试集上面的很多数据都是没有见过的，相比于训练数据，难免会有差别，故而如果用一个过拟合的模型，在测试集上面的表现自然不好。

我们称这样的模型“太过复杂”了（注意是引号），复杂的体现在于，模型中求出的参数在训练的时候为了“迎合”误差的减少，很多参数很大（过分强调一些特征），很多参数有很小（一些微小的特征），这样就会导致模型会有一种“偏爱”。自然，因为模型的这点偏爱，导致的结果就是，测试集的数据稍有波动，表现就不好了，即所谓的过拟合。与其说是“复杂”，倒不如说是“畸形”更恰当。





----

版权声明：本文为CSDN博主「LoveMIss-Y」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_27825451/article/details/83781270