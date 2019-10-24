---
title: 2019-10-24-NLP-gensim的doc2bow实现词袋模型
tags: python,
author:  Valuebai
---

词袋模型不做过多介绍，直接来个案例

```python
# 载入中文数据以及对应的包，corpora是构造词典的, similarities求相似性可以用得到。
from gensim import corpora, models, similarities
raw_documents = [  
    '0无偿居间介绍买卖毒品的行为应如何定性',  
    '1吸毒男动态持有大量毒品的行为该如何认定',  
    '2如何区分是非法种植毒品原植物罪还是非法制造毒品罪',  
    '3为毒贩贩卖毒品提供帮助构成贩卖毒品罪',  
    '4将自己吸食的毒品原价转让给朋友吸食的行为该如何认定',  
    '5为获报酬帮人购买毒品的行为该如何认定',  
    '6毒贩出狱后再次够买毒品途中被抓的行为认定',  
    '7虚夸毒品功效劝人吸食毒品的行为该如何认定',  
    '8妻子下落不明丈夫又与他人登记结婚是否为无效婚姻',  
    '9一方未签字办理的结婚登记是否有效',  
    '10夫妻双方1990年按农村习俗举办婚礼没有结婚证 一方可否起诉离婚',  
    '11结婚前对方父母出资购买的住房写我们二人的名字有效吗',  
    '12身份证被别人冒用无法登记结婚怎么办？',  
    '13同居后又与他人登记结婚是否构成重婚罪',  
    '14未办登记只举办结婚仪式可起诉离婚吗',  
    '15同居多年未办理结婚登记，是否可以向法院起诉要求离婚'  
]  

```



```python
# 将词语进行分词，并进行存储。
texts = [[word for word in jieba.cut(document, cut_all=True)] for document in raw_documents]
```


```python

# 寻找整篇语料的词典、所有词，corpora.Dictionary。
dictionary = corpora.Dictionary(texts)
```


```python
# 建立语料
corpus = [dictionary.doc2bow(text) for text in texts]

```

【Me】https://github.com/Valuebai/

【参考】
1、出处：地址