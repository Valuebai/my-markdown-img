---
title: 2019-9-27未命名文件 
tags: python,
author:  [Valuebai](https://github.com/Valuebai/)
---


## PageRank算法简介：


![图 1 PageRank算法](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1569569319714.png)


假设我们有4个网页——w1，w2，w3，w4。这些页面包含指向彼此的链接。有些页面可能没有链接，这些页面被称为悬空页面。

![enter description here](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1569569411160.png)

- w1有指向w2、w4的链接

- w2有指向w3和w1的链接

- w4仅指向w1

- w3没有指向的链接，因此为悬空页面


为了对这些页面进行排名，我们必须计算一个称为PageRank的分数。这个分数是用户访问该页面的概率。


为了获得用户从一个页面跳转到另一个页面的概率，我们将创建一个正方形矩阵M，它有n行和n列，其中n是网页的数量。

![enter description here](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1569569472985.png)



矩阵中得每个元素表示从一个页面链接进另一个页面的可能性。比如，如下高亮的方格包含的是从w1跳转到w2的概率。


![enter description here](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1569569512404.png)




**如下是概率初始化的步骤：**

1. 从页面i连接到页面j的概率，也就是M[i][j]，初始化为1/页面i的出链接总数wi 

2. 如果页面i没有到页面j的链接，那么M[i][j]初始化为0

3. 如果一个页面是悬空页面，那么假设它链接到其他页面的概率为等可能的，因此M[i][j]初始化为1/页面总数



因此在本例中，矩阵M初始化后如下：

![enter description here](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1569569625983.png)



最后，这个矩阵中的值将以迭代的方式更新，以获得网页排名。


## TextRank算法简介

**现在我们已经掌握了PageRank，让我们理解TextRank算法。我列举了以下两种算法的相似之处：**

- 用句子代替网页

- 任意两个句子的相似性等价于网页转换概率

- 相似性得分存储在一个方形矩阵中，类似于PageRank的矩阵M


TextRank算法是一种抽取式的无监督的文本摘要方法。让我们看一下我们将遵循的TextRank算法的流程：




1. 第一步是把所有文章整合成文本数据

2. 接下来把文本分割成单个句子

3. 然后，我们将为每个句子找到向量表示（词向量）。

4. 计算句子向量间的相似性并存放在矩阵中

5. 然后将相似矩阵转换为以句子为节点、相似性得分为边的图结构，用于句子TextRank计算。

6. 最后，一定数量的排名最高的句子构成最后的摘要。


【Me】https://github.com/Valuebai/

【参考】
1、出处：地址