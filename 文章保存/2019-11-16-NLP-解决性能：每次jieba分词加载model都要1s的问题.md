---
title: 2019-11-16-NLP-解决性能：每次jieba分词加载model都要1s的问题
tags: python,
author:  Valuebai
---

## 性能问题——加载jieba分词的model需要1s左右

性能指标：在初次打开阶段时间较长，后续逐渐变好，所以这是为啥呢？
——已经定位原因，首次加载jieba分词时loading了1.309s导致的
```md
Building prefix dict from the default dictionary ...
Dumping model to file cache C:\Users\AppData\Local\Temp\jieba.cache
Loading model cost 1.309 seconds.
Prefix dict has been built succesfully.
```

解决：
- 如果不希望每次都加载词库，可以让jieba初始化后再后台一直运行：
- 比如在flask中使用的时候应该在初始化app文件中初始化jieba，然后其他程序再调用初始化后的，这个之后讲flask的时候会讲到
```md
jieba 采用延迟加载，import jieba和 jieba.Tokenizer()不会立即触发词典的加载，
一旦有必要才开始加载词典构建前缀字典。如果你想手工初始 jieba，也可以手动初始化。

#!/usr/bin/python
# -*- coding: UTF-8 -*-

import jieba
jieba.initialize()

```


P.S. 另外的原因

这个服务器是海外的，也会影响数据的返回
http://139.180.217.25:8188/TextSummarization/ 

开课吧提供的阿里云服务器，则是正常的
http://39.100.3.165:8188/TextSummarization/

Best regards,
Huibo Lu


【Me】https://github.com/Valuebai/

【参考】
1、在分布式环境Spark中关闭jieba延时加载等优化方法 （3）：https://blog.csdn.net/macanv/article/details/87860691
2、jieba延迟加载问题解决：https://blog.csdn.net/yjs17125/article/details/81739382