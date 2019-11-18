---
title: 2019-11-18-梯度优化算法SGD、Momentum、Adagrad、Adadelta、Adam
tags: python,
author:  Valuebai
---

#### 学习梯度优化算法

这篇博客是梯度优化算法综述，附上一些读后感。
1. SGD : 每次朝着梯度的反方向进行更新。
2. Momentum：动能积累，每次更新时候积累 SGD 的更新方向，也就是每次参数更新时，不单单考虑当前的梯度方向，还考虑之前的梯度方向，当然之前的梯度会有一个衰减因子。
3. Adagrad：自动调节学习率大小，对于频繁更新的参数，学习率小；对于很少更新的参数，学习率大。怎么判断参数频繁更新呢？可以为更新公式设置一个分母：梯度平方累加和的平方根。这样当分母越大，说明之前该参数越频繁更新，反之则很少更新。存在问题：当迭代次数多了之后，由于是累加操作，分母越来越大，学习率会变得非常小，模型更新会很慢。
4. Adadelta：作为 Adagrad 的拓展，是解决学习率消失的问题。在这里不会直接叠加之前所有的梯度平方，而是引入了一个梯度衰退的因子，使得时间久远的梯度对此刻参数更新的影响会消失。RMSProp 的思想和其类似。
5. Adam：结合 Momentum 和 Adadelta 两种优化算法的优点。对梯度的一阶矩估计（First Moment Estimation，即梯度的均值）和二阶矩估计（Second Moment Estimation，即梯度的未中心化的方差）进行综合考虑，计算出更新步长。

文章地址：http://ruder.io/optimizing-gradient-descent/


【Me】https://github.com/Valuebai/

【参考】
1、出处：地址