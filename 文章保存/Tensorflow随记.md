---
title: Tensorflow随记
tags: python,
author:  [Valuebai](https://github.com/Valuebai/)
---

``` python
二、TensorFlow的基础运算
在搞神经网络之前，先让我们把TensorFlow的基本运算，也就是加减乘除搞清楚。

首先，TensorFlow有几个概念需要进行明确：

1 图（Graph）：用来表示计算任务，也就我们要做的一些操作。

2 会话（Session）：建立会话，此时会生成一张空图；在会话中添加节点和边，形成一张图，一个会话可以有多个图，通过执行这些图得到结果。如果把每个图看做一个车床，那会话就是一个车间，里面有若干个车床，用来把数据生产成结果。

3 Tensor：用来表示数据，是我们的原料。

4 变量（Variable）：用来记录一些数据和状态，是我们的容器。

5 feed和fetch：可以为任意的操作(arbitrary operation) 赋值或者从其中获取数据。相当于一些铲子，可以操作数据。

形象的比喻是：把会话看做车间，图看做车床，里面用Tensor做原料，变量做容器，feed和fetch做铲子，把数据加工成我们的结果。
```



【Me】https://github.com/Valuebai/

【参考】
1、出处：地址