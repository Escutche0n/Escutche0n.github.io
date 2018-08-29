---
layout:     post
title:      Learn Animations
subtitle:   Good animations enhance, bad animations distract
date:       2018-08-29
author:     EC
header-img: img/post-bg-designcode-animation.jpg
catalog: true
tags:
    - 设计
    - 动画
---



> UI 设计的简化让我们专注于动画本身。

### Introduction 入门

动画应该是用来增强 App 体验感的。它们让你的 App 更激励人心也更具动感。好的设计应该在点击和手势操作上**提供反馈** (**provide feedback**)，并给人以一种**直观操作**(**direct manipulation**)的沉浸感。

|     好的动画     |       坏的动画       |
| :--------------: | :------------------: |
|     快而流畅     |    慢而分散注意力    |
|   给交互以反馈   |        不必要        |
| 直观操作的沉浸感 | 与其他动画逻辑相违背 |
| 将动作高度视觉化 |       困扰用户       |

### Basics 基础

我们有一些基本的动画技巧来从A点到B点的过渡。这些动画极其普遍于每一个平台，从 Web 到 iOS。

通常来说，你不需要设置超过 2 个动画状态 (animation states)：**开始** 和 **结束**。计算机将会知道在这儿这之间发生了什么。

##### TRANSLATE 平移

这是一个典型的从位置 X 到 Y 的移动。 随着时间的改变，平移被用来 **滑动** (**sliding**)，**弹动** (**bouncing**) 和**抖动** (**shaking**) 视觉元素。

![translation](https://s1.ax1x.com/2018/08/29/POxYo6.gif)

##### ROTATE 旋转

通过改变元素的角度，你可以创造出有趣的效果，例如在 iOS 中的加载动画，或者是**添加** (**add**) 图标转化成**关闭** (**Close**) 图标。

![rotation](https://storage7.cuntuku.com/2018/08/29/0hW9z.gif)

##### SCALE 缩放

这个动画遍布于整个 iOS，例如 App 图标可以放大至 App 之中，或者用指尖去放大照片来布满整个屏幕。

![scale](https://media.giphy.com/media/51Y4uDK4J2iXsK6m5S/giphy.gif)

#### Animation Curve 动画曲线

曲线是让一个动画生动的关键一环 (<u>life-like</u>)。好的动画能复制真实的物理场景。一个地上弹起的球比一个平静掉落的更有趣。比起线性动画，现代 App 更倾向于用弹簧 (Spring) 和加速 (Ease) 动画。

![curve](https://media.giphy.com/media/u47vZLOdCk5CWJ96DJ/giphy.gif)

#### 3D Transform 3D 变换

当改变位置的时候，你也可以变换 X Y Z 三轴的值。虽然通常来讲，更少见也更难操控，但 3D 变换可以得到一个更有趣的结果让你的 App 脱颖而出。

![3d-transform](https://media.giphy.com/media/d5vw0ay440iIhcEdUr/giphy.gif)

#### Property Change 特性改变

一个非常流行的动画技巧就是改变 **颜色 **( **colour** )和 **透明度** ( **opacity** ) **属性**。只需在淡入淡出特效中修改透明度，这尤其的常见。在图层、文字和背景颜色上做改变也能产生有效的过渡。

![property-change](https://media.giphy.com/media/1yMzoIij6pZ0qCjJe3/giphy.gif)

#### Combining Animations 组合动画

基本上每一个场合你都需要将 2 到 3 个变换同时组合到一起。这样的话你能得到有趣的生活中的物理效果，如**弹跳** (**Bounce**)，**渐变 **(**Morph**) 和**抖动**(**Shake**)等。

来自 Tubik Studio 的 [动画](https://dribbble.com/shots/2148431-GIF-for-the-Timeline-App)

![combination](https://media.giphy.com/media/YiJnuvH0ZMKclWWdFW/giphy.gif)

#### 动画不要长于 1 秒

一个长的动画可以导致困惑 —— 它能让一个任务的效率变低。理想的持续时间通常为 **0.3 - 1 秒**。如果你用一个 Fade In，Slide 或者是 Scale， 这个时长就应该是你的目标。一个更短的时间会让它看起来没有动画，或者更糟的是它能给你的用户带来压力。一个更长的动画可能让它看起来表现不佳并挡道其真正的目的。

> Design is the **fundamental soul** of a human-made creation that ends up expressing itself in successive outer layers of the product or service. – Steve Jobs

Jakub Antalík 的 [动画](https://dribbble.com/shots/1949368-Sidebar-animation)

![animation](https://media.giphy.com/media/46zr9CKSUsy7Mh9rD3/giphy.gif)

### 和开发者合作

学习这些动画能让你更好的理解它们背后的技巧。你会一开始困惑如何得到最终的产品。所以在这一过程中向你的工程师们寻求帮助是很重要的。一个好的原型可以合理的解释你的动画概念。

### 动画工具

你有一系列的工具可以选择。有一些要求零基础但其他的会有一点陡峭的学习曲线。但回报是远大于付出的。更难领悟它，意味着你的 App 将会比别人的更优秀。

- Principle
- Flinto for Mac
- Origami
- Framer
- After Effects



"Design and Code Chapter 1 Learn Animation"

Tweet "Learn Animations - Good animations enhance, bad animations distract by @MengTo"