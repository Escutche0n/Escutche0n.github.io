---
layout:     post
title:      我的 VSCO Film 工作流
subtitle:   "VSCO Film - My Photography Workflow"
date:       2018-11-03
author:     "EC"
header-img: "img/post-bg-lightroom.jpg"
catalog: true
comments: true
tags:
    - 摄影
    - 工作流
    - Lightroom
    - VSCO Film
---

### Intro

今天写一篇关于如何使用 Lightroom 以及 VSCO Film 系列胶片滤镜的流程，希望大家喜欢。需要用到的工具为 Windows / Mac 版本的 Photoshop Lightroom Classic CC (之后简称 **Lightroom** )，以及一套 **VSCO Film** (或你喜爱的) 滤镜。

Lightroom 是当今数码拍摄之中不可或缺的一个步骤，与同为 Adobe 公司旗下的 Photoshop 相得益彰，然而两者的作用不太相同 。Lightroom 更偏向于对图片的光影和颜色进行**统一**的处理，对于风光类和食物类照片可以直接用 Lightroom 修片。Photoshop 则是在其基础上添加了 **图层 Layer** 和 **笔刷 Brush** 等概念，典型的应用场景为将多张图片合成，或者在人像类图片中对其脸部进行局部的修缮。

简而言之 Lightroom 更接近于如今移动端的各类 “P图软件”，有诸如曝光，阴影，色调，曲线等功能，也可以导入丰富的滤镜进行一键美化，并快速导出需要的照片。滤镜在 Lightroom 里被称作 **预设 preset**，在这万千滤镜中，**VSCO Film** 作为胶片滤镜的代表独树一帜。VSCO Film 按照不同的胶片命名，其精准的还原能力使得胶片的色彩感得以在数码的世界里体现的淋漓尽致。

[VSCO Film](http://vsco.co) 截至目前分为七个系列，其区别可以参考知乎 [VSCO Film 01 到 05 的区别是什么？](https://www.zhihu.com/question/23913317)

以下为 Lightroom 和 VSCO Film 的安 (pò) 装 (jiě) 简述，点此跳过废话直入正文。

---

Adobe 家全家桶的安装很简单，网络上有一大堆教程很好查找。我的使用的是在 [Adobe Creative Cloud](https://www.adobe.com/creativecloud/catalog/desktop.html) 的官网下载最新的适用版本，开始免费试用 (**Start free trial**)，然后用 Adobe Zii 加以破解，完成。Adobe Zii 可以破解整个Adobe系列的所有产品，如 Lightroom，Photoshop 以及我们耳熟能详的 Ai，AE等数十个艺术创作工具，截至目前CC 2018系列是可以破解的。



Adobe Zii   [百度云](https://pan.baidu.com/s/1o8E9za6) 密码 <u>fvtu</u>

VSCO Film [百度云](https://pan.baidu.com/s/1jxJdsC1nk_JQn31ZnbDR5g)  密码 <u>wr5l</u>

---

### 正文

##### 文件管理

无论是怎样的相机，在按下快门的那一刻，照片会统一存放在SD卡的某一个文件夹里并默认按照历史快门数进行命名，例如我的相机是松下的GH5s。例如 `P1015150.RW2` 和 `P1015150.jpg` 就分别代表着这部相机的第 15150 张照片的两个版本，前缀 `P` 为 松下 Panasonic 的首字母，其后的 `10` 是一个标签，`RW2` 和 `jpg` 则代表着两个版本，其中 `jpg` 是我们常见的有损图片格式，`RW2` 是松下的原始图像文件 (raw image file，之后简称 RAW)。

各个相机品牌之间 RAW 文件命名方式不同，如松下的 `RW2`，佳能的 `CR2`， 尼康的 `DNG`，以及索尼的 `ARW` 等。大多数厂牌的 RAW 可以按文件夹直接导入 Lightroom 处理图像。通常意义上同一张照片的 RAW 图像会比传统的 jpg 有着更高的宽容度，意味着其保留着更多画面细节的同时带来了更广阔的后期空间，RAW 的成像素质取决于相机机身，手机照片RAW。 

我习惯将所有照片存放在一个图片文件夹中，按照**机型**分类，子文件夹按**时间**分类，如果有一天遇到了两次不同的拍摄，则第二次的日期后缀会 `-` 增加 brief 加以区分。

![](https://ws2.sinaimg.cn/large/006tNbRwgy1fwvg9tkxgej31gw0zy7d8.jpg)

在做好了文件管理的步骤之后，才算是开始了正式的图片处理工作。

##### 导入及筛选

![Lightroom Loading Page](https://ws3.sinaimg.cn/large/006tNbRwgy1fwvgtstvn9j31ao0tmdng.jpg)

Lightroom Classic 2018 的加载画面，完成后进入 Lightroom 主界面。

![](https://ws3.sinaimg.cn/large/006tNbRwgy1fwvh1r1nqnj31kw0zk1gv.jpg)

进入主界面之后右上角有两个页面切换键，**图库 Library** 和 **修改照片 Develop**，右键点击这两个标签我们可以隐藏其余的 **地图 Map** 等选项，其中 **图库** 是用来浏览所有照片的窗口，在其中导入了照片之后，我通常会在这里将所有大片浏览一遍，将可用的照片标记出来，进行简单的设置以及打分。

左侧边栏的文件夹选项可以点击 **`+`** 图标添加我们整理好的文件夹导入我们的照片，也可以在文件夹处点击右键，选择同步文件夹，都可以添加新的照片。

右侧边栏显示了直方图，快速修改照片，元数据等选项，我们可以点击色调控制的 **自动** 按钮来调整照片的亮度，如果是 RAW 还可以更改白平衡设置，使得所有照片达到一个相对平衡的水平。

中间的图片过滤器展示了所有的照片，在这里我们可以切换网格视图 **`G`** 与放大视图 **`E`** ，在放大视图中显示单一图片或在网格视图中选中图片可以对其**评分 rating**， 前期素材按照优劣进行打分，快捷键 **`1`** - **`5`** 打星，如果照片确定了彻底不可用可以右键进行标记 **`排除`**。快捷键 **`6`** - **`9`** 则是用来按颜色标记，我通常用不同的颜色标记一次拍摄中预计不同风格的两组照片，或者是我和朋友分别喜欢的照片，在下方过滤器中选择了照片之后，会如下图。

![](https://ws3.sinaimg.cn/large/006tNbRwgy1fwvokrvkluj31kw0zkwzr.jpg)

##### 使用预设

在完成了图片的修饰工作之后，开始修改照片。

首先选择合适的预设，我个人偏好 VSCO 05 ( Archetype Films ) 系列的 **Kodak Gold 100** 预设。

![VSCO 05](https://drh1.img.digitalriver.com/DRHM/Storefront/Company/vsco/images/product/detail/05_PPD%20Hero_2x.jpg)

##### 调整基本参数和色调曲线

这张图中我选择的是 **Kodak Gold 100 + Soft Highs**，在 **裁剪** 至 4:5 之后轻微调整了**曝光 Exposure**、**高光 Highlights**、**阴影 shadows**、**污点去除工具** 等参数。

![](https://ws3.sinaimg.cn/large/006tNbRwgy1fwvrl1z8nvj31kw0zk1kx.jpg)

ik pm

##### 引导式变换

将右边栏往下划过 **HSL / 颜色** ，**细节** 和**镜头校正**，一直到最后的 **变换** 和 **效果** 。

跳过的三个菜单效果如下：

- HSL / 颜色 ：在人像片中肤色调教时需要对 **橙色** 进行特别的矫正。
- 细节：在需要的时候适当 **锐化 **和 **降噪** 。
- 镜头校正：除松下外主流镜头均有 **配置文件**（包括  iPhone） 以矫正镜头自带暗角与畸变。

在变幻一栏中选择 **引导式变换** ，此时可以在照片中确定四条纵横标准线，我选择的分别是左边的路灯，右边吊塔到桥上，中间市政厅，以及马路左右。选择相对距离较远的两对标准线可以让画面更有 “**正**” 的感觉，从而得到一张完美构图的照片。

最后的 **效果** 菜单中可以选择添加 **颗粒** 效果，若不需要过度的胶片感可以减弱效果， 因为VSCO Film 各类滤镜自带不同程度的颗粒。

![](https://ws1.sinaimg.cn/large/006tNbRwgy1fwvsrrojzaj31kw0zk1kx.jpg)



##### 导出照片

最后选定需要的图片，选定图像格式 `JPEG` 和色彩空间 `sRGB` 导出，我通常会限制文件大小为 500KB 并用 Image Optim 压缩以方便 web 传播。

![](https://ws2.sinaimg.cn/large/006tNbRwgy1fwvt46w9vzj31kw0whgri.jpg)

---

![](https://ws1.sinaimg.cn/large/006tNbRwgy1fwvtao9zlgj31kw0zk1kx.jpg)

### 结语

最后，衷心的祝贺看到文章结尾的各位都能拍到喜欢的照片。