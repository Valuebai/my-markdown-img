---
title: 2019-11-11-NLP-Python中的scale函数和MinMaxScaler()函数
tags: python,
author:  Valuebai
---

# 一. 标准化Standardization（这里指移除均值和方差标准化） 
标准化是很多数据分析问题的一个重要步骤，也是很多利用机器学习算法进行数据处理的必要步骤。

## 1.1 z-score标准化 

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

## 1.2 transform

这个类实现了一个叫做Transformer的应用程序接口，能够计算训练数据的均值和标准差，从而在训练数据集上再次使用。

```python

from sklearn import preprocessing
X=[[1.,-1.,2.],
       [2.,0.,0.],
       [0.,1.,-1.]]
scaler = preprocessing.StandardScaler().fit(X)
print scaler
print scaler.mean_ #每列的平均数
print scaler.scale_   #缩放比例
print scaler.transform(X)
scaler = preprocessing.StandardScaler().fit(X)
print scaler
print scaler.transform([[-1., 1., 0.]])  # 在其他数据集上使用
```

一般会把train和test集放在一起做标准化，或者在train集上做标准化后，用同样的标准化器去标准化test集，此时可以用scaler

实际应用中，需要做特征标准化的常见情景：SVM


## 1.3 将特征数据缩放到一个范围 scale to a range 
利用最大值和最小值进行缩放，通常是将数据缩放到0-1这个范围，或者是将每个特征的绝对值最大值缩放到单位尺度，分别利用MinMaxScaler和MaxAbsScaler实现。 
使用这一方法的情况一般有两种： 
(1) 特征的标准差较小 
(2) 可以使稀疏数据集中的0值继续为0

### 1）MinMaxScaler   计算方式是特征值减去最小值除以最大值减去最小值 

```python
from sklearn import preprocessing
X=[[1.,-1.,2.],
   [2.,0.,0.],
   [0.,1.,-1.]]
min_max_scaler = preprocessing.MinMaxScaler()
x_scaled_minmax = min_max_scaler.fit_transform(X)
print x_scaled_minmax
 
#可以查看缩放算子的一些属性:
#这个transformer的实例还能够应用于新的数据集，此时的缩放比例与之前训练集上的缩放比例是相同的。
x_test = np.array([[3., 1., 4.]])
print min_max_scaler.transform(x_test)
 
print min_max_scaler.scale_  # 缩放比例=1/(max-min)
print min_max_scaler.min_   # (x-min)/(max-min), 这里min_代表min/(max-min)
```

第一列的最大值为2，最小值为0，x_scaled_minmax第一列数据为：
（1-最小值）/(最大值 -最小值) =（1-0）/(2-0）=0.5

（2-最小值）/(最大值 -最小值) =（2-0）/(2-0）=1

（0-最小值）/(最大值 -最小值) =（0-0）/(2-0）=0


同理：min_max_scaler  也是如此，同样的标准化器去标准化 x_test集[3., 1., 4.]
min_max_scaler  这一行数据为： 1.5         1.          1.66666667
第一列的最大值为2，最小值为0，
（3-最小值）/(最大值 -最小值) =（3-0）/(2-0）=1.5
第二列的最大值为1，最小值为-1
（1-最小值）/(最大值 -最小值) =（1- （-1））/(1-（-1））=1
第三列的最大值为2，最小值为-1
（4-最小值）/(最大值 -最小值) =（4-（-1））/(2-（-1））=1.66666667

MaxAbsScaler与上述用法相似，但是标准化后的数据的取值范围为[-1, 1]。这对于稀疏数据或者是数据中心已经为0的数据很有意义。
————————————————
版权声明：本文为CSDN博主「liulina603」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/liulina603/article/details/78791198



【Me】https://github.com/Valuebai/

【参考】
1、出处：地址