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

隐含狄利克雷分布(LDA)


根据定义，LDA是给定语料库的生成概率模型。其基本思想是将文档表示为潜在主题的随机混合，并对每个主题通过单词分布进行特征化。



LDA的底层算法


【Me】https://github.com/Valuebai/

【参考】
1、出处：地址