---
title: 使用中文维基百科语料库训练一个word2vec模型 
tags: python,
author: Valuebai
---
本文章默认在python3.7+上运行


欢迎使用 **{小书匠}(xiaoshujiang)编辑器**，您可以通过 `小书匠主按钮>模板` 里的模板管理来改变新建文章的内容。

注意点：复制图片过来，要先等一会，要先等一会，要先等一会，让小书匠保存成github的图床图片，先不要保存到github上的文章，不然图片获取用的是：(./images/1561707358500.png)，会导致复制到CSDN上无法使用



本篇文章主要介绍如何通过中文维基百科语料库来训练一个word2vec模型。

相关资料下载：

中文维基百科下载地址：

https://dumps.wikimedia.org/zhwiki/20190720/  （我用的是这个，用UC浏览器下载）

https://dumps.wikimedia.org/zhwiki/  （在这里下载最新的）

WikiExtractor项目git地址：https://github.com/attardi/wikiextractor  

OpenCC项目git地址：https://github.com/BYVoid/OpenCC

中文分词jieba项目git地址：https://github.com/fxsjy/jieba

gensim官网地址：https://radimrehurek.com/gensim/install.html




#### 一、语料库的下载
我下载是zhwiki-20190720-pages-articles-multistream.xml.bz2文件，1.5G左右是一个压缩包，下载的时候需要注意文件的名称。
https://dumps.wikimedia.org/zhwiki/20190720/  （我用的是这个，用UC浏览器下载）

#### 二、语料库文章的提取
下载完成之后，解压缩得到的是一个xml文件，里面包含了许多的文章，也有许多的日志信息。所以，我们只需要提取xml文件里面的文章就可以了。我们通过WikiExtractor来提取xml文件中的文章，它是一个意大利人写的一个Python脚本专门用来提取维基百科语料库中的文章，将每个文件分割的大小为500M（或者2000M）（修改下面的命令），它是一个通过cmd命令来设置一些参数提取文章，提取步骤如下：

##### a、WikiExtractor的安装

将整个WikiExtractor项目clone或者下载到本地，拷贝里面的WikiExtractor.py出来，放到跟上面的xxx.bz2同一个文件夹即可（2019-07-30 github里面没有setup.py，有的教程说要用，直接拿来用就行啦）

##### b、维基百科语料库文章的提取
windows下cmd进入路径，包含下面2个的路径：
WikiExtractor.py和zhwiki zhwiki-20190720-pages-articles-multistream.xml.bz2

``` javascript
python WikiExtractor.py -b 2000M -o zhwiki zhwiki-20190720-pages-articles-multistream.xml.bz2

python WikiExtractor.py -b 500M -o zhwiki zhwiki-20190720-pages-articles-multistream.xml.bz2

# 分割的大小为500M（或者2000M）
```
参数介绍：

-b，设置提取文章后的每个文件大小

-o，制定输出文件的保存目录

zhwiki-20180720-pages-articles.xml，下载的维基百科语料库文件

更多参数的使用，可以通过以下命令查看：

``` javascript
python WikiExtractor.py -h
```
![enter description here](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1564459870393.png)

使用WikiExtractor提取文章，会在指定目录下产生一个AA的文件夹，里面会包含很多的文件。使用WikiExtractor提取的文章格式如下：

![enter description here](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1564459979320.png)

其中省略号表示的就是文章的内容，所以后面我们还需要通过正则化表达式来去除不相关的内容。

##### c、中文简体和繁体的转换

因为维基百科语料库中的文章内容里面的简体和繁体是混乱的，所以我们需要将所有的繁体字转换成为简体。这里我们利用OpenCC来进行转换。

OpenCC的github地址： https://github.com/BYVoid/OpenCC

OpenCC的使用教程请参考：https://blog.csdn.net/sinat_29957455/article/details/81290356

##### d、正则表达式提取文章内容并进行分词

使用WikiExtractor提取的文章，会包含许多的<doc></doc>，所以我们需要将这些不相关的内容通过正则表达式来去除。然后再通过jieba对文章进行分词，在分词的时候还需要将一些没有实际意义的词进行去除，所以在分词的之后加了一个停用词的去除。将分割之后的文章保存到文件中，每一行表示一篇文章，每个词之间使用空格进行分隔。



【Me】https://github.com/Valuebai/

【参考】
1、出处：地址