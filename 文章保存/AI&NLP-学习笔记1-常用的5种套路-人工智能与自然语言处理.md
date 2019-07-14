---
title: AI&NLP-学习笔记1-常用的5种套路-人工智能与自然语言处理
tags: python,AI,NLP
author: Valuebai
---
本文章默认在python3.7+上运行，不支持py2

欢迎使用 **{小书匠}(xiaoshujiang)编辑器**，您可以通过 `小书匠主按钮>模板` 里的模板管理来改变新建文章的内容。
注意点：先不要保存到github上面，不然图片获取用的是：(./images/1561707358500.png)，会导致复制到CSDN上无法使用

用计算机在处理自然语言（NLP, Natual Language Processing）的过程中，经常用到的5种Paradigm（用现在的话讲，就是“套路”）
1. Rule Based 
2. Probability Based
3. Mathmatical and Analytic Based
4. Search Based
5. Machine Learning(Deep Learning)

上面这几个是常用的套路，没有好坏之分，只是看在哪个场景下更加适用，这几年最火的是基于机器学习/深度学习来做 的
1. 基于规则的，从人类语言抽象出来：主语-谓语-宾语，来创造句子——这里主要关注syntaxtree
2. 基于概率的，上面Rule Based生成的句子经常四不像，怎么判断一个生成的句子好坏，就得提到【语言模型】用来看生成的句子是否符合人们的说话习惯——这里主要关注1-GRAM,2-GRAM,3-GRAM,4-GRAM...其他的N-GRAM了解下就行，用不上的
3. 基于数学和分析的
4. 基于搜索的，会涉及到DFS, BFS, 图, 动态规划, A star等算法


【Me】https://github.com/Valuebai/

【参考】
1、出处：地址