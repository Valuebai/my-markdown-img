---
title: 2019-10-21主题模型：LDA 隐含狄利克雷分布，用Python实现
tags: python,
author:  Valuebai
---

原文（英文）发表地址：[Topic Modeling in Python: Latent Dirichlet Allocation (LDA)](https://towardsdatascience.com/end-to-end-topic-modeling-in-python-latent-dirichlet-allocation-lda-35ce4ed6b3e0)

中文发表地址：[Python中的端对端主题建模: 隐含狄利克雷分布(LDA)](https://mp.weixin.qq.com/s?__biz=MjM5NzU0MzU0Nw==&mid=2651382805&idx=1&sn=440b18df09f2c9e67a6c025d4239093c&scene=0#wechat_redirect)


## Introduction
Topic Models, in a nutshell, are a type of statistical language models used for uncovering hidden structure in a collection of texts. In a practical and more intuitively, you can think of it as a task of:

**Dimensionality Reduction**, where rather than representing a text T in its feature space as {Word_i: count(Word_i, T) for Word_i in Vocabulary}, you can represent it in a topic space as {Topic_i: Weight(Topic_i, T) for Topic_i in Topics}

**Unsupervised Learning**, where it can be compared to clustering, as in the case of clustering, the number of topics, like the number of clusters, is an output parameter. By doing topic modeling, we build clusters of words rather than clusters of texts. A text is thus a mixture of all the topics, each having a specific weight

**Tagging**, abstract “topics” that occur in a collection of documents that best represents the information in them.

There are several existing algorithms you can use to perform the topic modeling. The most common of it are, **Latent Semantic Analysis (LSA/LSI), Probabilistic Latent Semantic Analysis (pLSA), and Latent Dirichlet Allocation (LDA)**


In this article, we’ll take a closer look at **LDA**, and implement our first topic model using the sklearn implementation in python 3.6.5

**简而言之**，主题模型: 它是一种统计模型，用于标记出现在文档集合中的抽象“主题”，这些主题最能代表这个文档集合中的信息。


获取主题模型使用了许多技术，如：Latent Semantic Analysis (LSA/LSI), Probabilistic Latent Semantic Analysis (pLSA), and Latent Dirichlet Allocation (LDA)

**本文主要结合LDA+sklearn 来实现，内容通俗易懂~~**


## Theoretical Overview

**根据定义，LDA是给定语料库的生成概率模型。其基本思想是将文档表示为潜在主题的随机混合，并对每个主题通过单词分布进行特征化。**
![LDA的底层算法, http://chdoig.github.io/pytexas2015-topic-modeling/#/3/4](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1571642518975.png)

We can describe the generative process of LDA as, given the M number of documents, N number of words, and prior K number of topics, the model trains to output:
psi, the distribution of words for each topic K
phi, the distribution of topics for each document i

我们可以把LDA的处理过程进行如下描述，给定M个文档、N个单词和估计的K个主题, LDA模型训练输出的结果：
- psi，表示每个主题K的单词分布
- phi，表示每个文档 i 的主题分布。

## Parameters of LDA
> **Alpha parameter** is Dirichlet prior concentration parameter that represents document-topic density — with a higher alpha, documents are assumed to be made up of more topics and result in more specific topic distribution per document.
> **Beta parameter** is the same prior concentration parameter that represents topic-word density — with high beta, topics are assumed to made of up most of the words and result in a more specific word distribution per topic.

> **α参数**是Dirichlet先验浓度参数,表示文档-主题密度，α值越高，就可以假定文档由更多的主题组成，从而导致每个文档的主题分布更加具体。
> **β参数**与先验浓度参数相同,表示主题词密度的，β值越高，就可以假定主题由大部分单词组成，从而导致每个主题的单词分布更加具体。

## LDA Implementation

[LDA的代码实现，具体看github: https://github.com/kapadias/mediumposts/blob/master/nlp/published_notebooks/Introduction%20to%20Topic%20Modeling.ipynb](https://github.com/kapadias/mediumposts/blob/master/nlp/published_notebooks/Introduction%20to%20Topic%20Modeling.ipynb)

有时候打不开，可以用https://nbviewer.jupyter.org 进行加载

1. Loading data 加载数据

2. Data cleaning 数据清洗
 
3. Exploratory analysis 探索性分析

4. Preparing data for LDA analysis 为LDA分析准备数据

5. LDA model training LDA模型训练

6. Analyzing LDA model results 分析LDA模型结果


## Loading data 加载数据

在本教程中，我们将使用NIPS大会上发表的论文数据集。NIPS大会(神经信息处理系统)是机器学习领域最负盛名的年度事件之一。在每次的NIPS大会上，参会者都会发表大量的研究论文。此CSV文件包含了从1987年到2016年(29年!)发表的不同的NIPS论文的信息。这些论文讨论了机器学习领域的各种各样的主题，从神经网络到优化方法等等。

首先，我们将研究CSV文件，以确定我们可以使用什么类型的数据进行分析，以及它的结构是怎样的。一篇研究论文通常由标题、摘要和正文组成。

```python
# Importing modules
import pandas as pd
import os
os.chdir('..')
# Read data into papers
papers = pd.read_csv('./data/NIPS Papers/papers.csv')
# Print head
papers.head()

```

【Me】https://github.com/Valuebai/

【参考】
1、出处：地址