---
title: 2019-11-11-NLP-Python中的scale函数和MinMaxScaler()函数
tags: python,
author:  Valuebai
---

一. 标准化Standardization（这里指移除均值和方差标准化） 
标准化是很多数据分析问题的一个重要步骤，也是很多利用机器学习算法进行数据处理的必要步骤。

1.1 z-score标准化 

z-score标准化指的是将数据转化成均值为0方差为1的高斯分布，也就是通常说的z-score标准化，但是对于不服从标准正态分布的特征，这样做效果会很差。

在实际应用中，我们经常忽视分布的形状，将数据进行z-score标准化。如果不将数据进行标准化处理，在利用机器学习算法（例如SVM）的过程中，如果目标函数中的一个特征的方差的阶数的量级高于其他特征的方差，那么这一特征就会在目标函数中占主导地位，从而“淹没”其他特征的作用。


Python中的scale函数是一种快速进行z-score标准化的方法，能够处理类似于数组结构的数据。Z-score标准化后的数据的均值为0，方差为1。


```python
from sklearn import preprocessing
X=[[1.,-1.,2.],
       [2.,0.,0.],
       [0.,1.,-1.]]
X_scaled = preprocessing.scale(X)
print X_scaled
print X_scaled.mean(axis=0)  #均值为0
print X_scaled.std(axis=0)   #方差为1
```









【Me】https://github.com/Valuebai/

【参考】
1、出处：地址