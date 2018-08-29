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



> The simplification of UI design enables the focus in animation. In this lesson, you'll learn the basic techniques to animations such as slide, scale and color transitions. Then, you'll learn about the more advanced techniques introduced in iOS for transitional interfaces and physics animations combined with gestures. 

### Introduction to Animations 动画入门

Animations should enhance the app experience. They make your app more engaging and dynamic. Good animations should **provide feedback** on taps and gestures, and give a sense of **direct manipulation**.

|          Good animation           |            Bad animation             |
| :-------------------------------: | :----------------------------------: |
|          Fast and fluid           |         Slow and distracting         |
|   Give feedback on interactions   |      Gratuitous and obstructive      |
|   Sense of direct manipulation    | Inconsistent wih built-in animations |
| Help visualise results of actions |            Confuse users             |

### Animation Basics 动画基础

There are a number of basic animation techniques that we use to transition from point A to point B. These animations are so common that they can be found of every platform, from Web to iOS.

Typically, you don’t need to set more than 2 animation states: **Begin** and **End**. The computer will know what happens in between those states.

##### TRANSLATE 平移

This is as simple as moving the position X and Y. With different timing, Translate is used for sliding, bouncing and shaking visual elements.

![translation](https://s1.ax1x.com/2018/08/29/POxYo6.gif)

##### ROTATE 旋转

By changing the angle of the element, you can create interesting effects like the loading animation in iOS, or the **Add** icon that turns into a **Close** icon.

![rotation](https://storage7.cuntuku.com/2018/08/29/0hW9z.gif)

##### SCALE 缩放

This animation pervades iOS, such as the app icon that zooms inside the app, or the photo thumbnails that expand to full-screen view.



### Animation Curve 动画曲线

The **curve** plays a big part in making the animation more <u>life-like</u>. Good animations replicate real physics. A ball that bounces on the ground makes a much more interesting subject than one that falls flat. Modern apps tend to use Spring and Ease animations much more than Linear.

常用的曲线有：

- Linear
- Ease In
- Ease Out
- Ease In Out
- Spring

### 3D Transform 3D 变换

When changing the position, you can also play with the X, Y and Z values. Although generally more rare and harder to pull, 3D transforms can give interesting results that can really set your app apart.

### Property Change 特性改变

A very popular animation technique is to change the **color** and **opacity properties**. Just modifying the opacity results in the Fade In / Fade Out effect, which is extremely common. Animating the layer, text and background colors can also yield effective transitions.

### Combining Animations 组合动画

On almost every occasion, you’ll want to work with 2 or 3 transforms at the same time. By doing so, you get interesting animations inspired by real life physics such as **Bounce**, **Morph**, **Shake**, etc.

### Animations Shouldn't Last Longer Than 1 second

A long animation can lead to frustration — it can get in the way of performing a task. The ideal duration that I’ve personally found is **0.3 - 1 second**. If you do a Fade In, Slide or Scale, that’s the duration that you should aim for. Anything shorter would make seem like there was no animation, or worse it could stress your users. Anything longer may seem underperforming and can be in the way of doing what they want.

> Design is the **fundamental soul** of a human-made creation that ends up expressing itself in successive outer layers of the product or service. – Steve Jobs

### Working with Developers

Learning these animations will allow you to understand the techniques behind them. You’ll struggle at first to get the results that you seek in the final product, so it’s important that you include your engineers in the process. A prototype will work wonders in explaining your animated concepts. In my personal experience, knowing different paths to a goal, from the simple options to the complex ones, will allow you to make a wise decision based on your timeframe.

### The Animation Tools

You have several animation tools to choose from. Some require little to no experience and others require a pretty steep learning curve. But the rewards are much greater. The harder it gets, the more likely your app will stand out from others.

- Principle
- Flinto for Mac
- Origami
- Framer
- After Effects