---
title: 2019-11-5-NLP-networkx 不能显示中文的解决办法 
tags: python,
author:  Valuebai
---

在windows的字体库中找个替换

修改matplotlib\mpl-data\matplotlibrc

font.family : sans-serif #打开该选项

将font.sans-serif 后面的DejaVu Serif改成SimHei

改后为font.sans-serif : SimHei, Bitstream Vera Sans, Computer Modern Sans Serif, Lucida Grande, Verdana, Geneva, Lucid, Arial, Helvetica, Avant Garde, sans-serif
注意去掉#





【Me】https://github.com/Valuebai/

【参考】
1、出处：地址