---
title: 2019-11-11-NLP-使用sklearn做单机特征工程
tags: python,
author:  Valuebai
---

1 特征工程是什么？
2 数据预处理
　　2.1 无量纲化
　　　　2.1.1 标准化
　　　　2.1.2 区间缩放法
　　　　2.1.3 标准化与归一化的区别
　　2.2 对定量特征二值化
　　2.3 对定性特征哑编码
　　2.4 缺失值计算
　　2.5 数据变换
　　2.6 回顾
3 特征选择
　　3.1 Filter
　　　　3.1.1 方差选择法
　　　　3.1.2 相关系数法
　　　　3.1.3 卡方检验
　　　　3.1.4 互信息法
　　3.2 Wrapper
　　　　3.2.1 递归特征消除法
　　3.3 Embedded
　　　　3.3.1 基于惩罚项的特征选择法
　　　　3.3.2 基于树模型的特征选择法
　　3.4 回顾
4 降维
　　4.1 主成分分析法（PCA）
　　4.2 线性判别分析法（LDA）
　　4.3 回顾
5 总结
6 参考资料

---

# 1 特征工程是什么？
　　有这么一句话在业界广泛流传：数据和特征决定了机器学习的上限，而模型和算法只是逼近这个上限而已。那特征工程到底是什么呢？顾名思义，其本质是一项工程活动，目的是最大限度地从原始数据中提取特征以供算法和模型使用。通过总结和归纳，人们认为特征工程包括以下方面：


![enter description here](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1573468408513.png)


特征处理是特征工程的核心部分，sklearn提供了较为完整的特征处理方法，包括数据预处理，特征选择，降维等。首次接触到sklearn，通常会被其丰富且方便的算法模型库吸引，但是这里介绍的特征处理库也十分强大！

　　本文中使用sklearn中的IRIS（鸢尾花）数据集来对特征处理功能进行说明。IRIS数据集由Fisher在1936年整理，包含4个特征（Sepal.Length（花萼长度）、Sepal.Width（花萼宽度）、Petal.Length（花瓣长度）、Petal.Width（花瓣宽度）），特征值都为正浮点数，单位为厘米。目标值为鸢尾花的分类（Iris Setosa（山鸢尾）、Iris Versicolour（杂色鸢尾），Iris Virginica（维吉尼亚鸢尾））。导入IRIS数据集的代码如下：
  
  ```python
  from sklearn.datasets import load_iris

#导入IRIS数据集
iris = load_iris()

#特征矩阵
iris.data

#目标向量
iris.target
```









【Me】https://github.com/Valuebai/

【参考】
1、出处：地址