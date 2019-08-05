---
title: 使用中文维基百科语料库训练一个word2vec模型 
tags: python, word2vec模型, wiki中文语料训练, gensim
author: Valuebai
---
本文章默认在python3.7+上运行

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
https://dumps.wikimedia.org/zhwiki/20190720/  （我用的是这个，windows用UC浏览器下载）

linux下载命令：
wget https://dumps.wikimedia.org/zhwiki/20190720/zhwiki-20190720-pages-articles-multistream.xml.bz2


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

# 分割的大小为500M（或者2000M）——词向量长度一般是100-300M哦

# 解压抽取词汇

python bzcat zhwiki-20190720-pages-articles-multistream.xml.bz2 | python WikiExtractor.py -b 500M -o extracted >output.txt  ——我执行后报can't open file 'bzcat': [Errno 2] No such file or directory，先不处理 了
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

##### c、中文简体和繁体的转换——高老师说不用这个，因为太繁琐了，使用pip install hanziconv

因为维基百科语料库中的文章内容里面的简体和繁体是混乱的，所以我们需要将所有的繁体字转换成为简体。这里我们利用OpenCC来进行转换。


OpenCC的使用教程请参考：https://blog.csdn.net/sinat_29957455/article/details/81290356


``` javascript
opencc -i 需要转换的文件路径 -o 转换后的文件路径 -c 配置文件路径
```

``` javascript
我碰到的问题：
1、用WikiExtractor.py提取后的路径：C:\Users\中文\Desktop\AI-NLP\learn-NLP-luhuibo\lesson-04\zhwiki500\AA\wiki_00

2、在这个路径执行：
C:\Users\中文\Desktop\AI-NLP\learn-NLP-luhuibo\lesson-04\zhwiki500\AA>opencc -i wiki_00 -o zh_wiki_00 -c C:\Users\中文\Desktop\AI-NLP\learn-NLP-luhuibo\lesson-04\opencc-1.0.4\share\opencc\t2s.json
——报错：t2s.json not found or not accessible.

在路径执行：C:\Users\壹心理\Desktop\AI-NLP\learn-NLP-luhuibo\lesson-04>opencc -i wiki_00 -o zh_wiki_00 -c opencc-1.0.4\share\opencc\t2s.json
——上面这个命令是OK的

# 解决方法（t2s.json not found or not accessible.）：

下载完成之后，解压到本地即可。解压之后可以将OpenCC下的bin目录添加到系统环境变量中。
——不要放到中文路径，直接放到C盘下，C:\opencc-1.0.4\share\opencc\t2s.json

正常的命令是：opencc -i wiki_00 -o zh_wiki_00 -c C:\opencc-1.0.4\share\opencc\t2s.json

```

##### d、正则表达式提取文章内容并进行分词

使用WikiExtractor提取的文章，会包含许多的<doc></doc>，所以我们需要将这些不相关的内容通过正则表达式来去除。然后再通过jieba对文章进行分词，在分词的时候还需要将一些没有实际意义的词进行去除，所以在分词的之后加了一个停用词的去除。将分割之后的文章保存到文件中，每一行表示一篇文章，每个词之间使用空格进行分隔。


##### e、将分词后的文件合并为一个

将分词后的多个文件合并为一个文件，便于word2vec模型的训练

d和e代码在这里：

``` javascript
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''=================================================
@IDE    ：PyCharm
@Author ：LuckyHuibo
@Date   ：2019/8/5 11:12
@Desc   ：使用中文维基百科语料库训练一个word2vec模型
根据这个教程改编的
https://blog.csdn.net/sinat_29957455/article/details/81432846
1、我已经用WikiExtractor.py提取完数据，并用opencc转为简体字，放在我本地的./data/目录下
2、代码中的文件路径，记得自己修改，文件太大我设置gitignore了
3、stopwords.txt停用词表，我放在同一目录下

==================================================

import logging, jieba, os, re


def get_stopwords():
    '''
    加载停用词表，去掉一些噪声
    :return:
    '''
    logging.basicConfig(format='%(asctime)s:%(levelname)s:%(message)s', level=logging.INFO)
    # 加载停用词表
    stopword_set = set()
    with open("stopwords.txt", 'r', encoding="utf-8") as stopwords:  # stopwords.txt停用词表，我放在同一目录下
        for stopword in stopwords:
            stopword_set.add(stopword.strip("\n"))
    return stopword_set


def parse_zhwiki(read_file_path, save_file_path):
    '''
    使用正则表达式解析文本
    '''
    # 过滤掉<doc>
    regex_str = "[^<doc.*>$]|[^</doc>$]"
    file = open(read_file_path, "r", encoding="utf-8")
    # 写文件
    output = open(save_file_path, "w+", encoding="utf-8")
    content_line = file.readline()
    # 获取停用词表
    stopwords = get_stopwords()
    # 定义一个字符串变量，表示一篇文章的分词结果
    article_contents = ""
    while content_line:
        match_obj = re.match(regex_str, content_line)
        content_line = content_line.strip("\n")
        if len(content_line) > 0:
            if match_obj:
                # 使用jieba进行分词
                words = jieba.cut(content_line, cut_all=False)
                for word in words:
                    if word not in stopwords:
                        article_contents += word + " "
            else:
                if len(article_contents) > 0:
                    output.write(article_contents + "\n")
                    article_contents = ""
        content_line = file.readline()
    output.close()


def generate_corpus():
    '''
    将维基百科语料库进行分类
    '''
    zhwiki_path = "./data/"  # 加载zhwiki的路径
    save_path = "./data/"  # 保存zhwiki的路径
    for i in range(3):
        file_path = os.path.join(zhwiki_path, str("zh_wiki_0%s" % str(i)))
        parse_zhwiki(file_path, os.path.join(save_path, "wiki_corpus0%s" % str(i)))


def merge_corpus():
    '''
    合并分词后的文件
    '''
    output = open("./data/wiki_corpus", "w", encoding="utf-8")
    input = "./data/"
    for i in range(3):
        file_path = os.path.join(input, str("wiki_corpus0%s" % str(i)))
        file = open(file_path, "r", encoding="utf-8")
        line = file.readline()
        while line:
            output.writelines(line)
            line = file.readline()
        file.close()
    output.close()


if __name__ == "__main__":
    generate_corpus()
    merge_corpus()

```

#### 三、word2vec模型的训练
训练word2vec模型的时候，需要使用到gensim库，安装教程请参考官网，通过pip命令就可以进行安装。训练过程需要30分钟到1个小时，具体训练时间与电脑的配置相关。

``` javascript
import logging
from gensim.models import word2vec
 
def main():
    logging.basicConfig(format="%(asctime)s:%(levelname)s:%(message)s",level=logging.INFO)
    sentences = word2vec.LineSentence("./data/wiki_corpus")		#注意替换成你的路径
    model = word2vec.Word2Vec(sentences,size=250)	# size默认是100-300，根据你的语料大小进行增加，效果看你的需求
    #保存模型
    model.save("model/wiki_corpus.model")		# 我的代码是保存为zhwiki_news.word2vec，需要你自己改下
	
if __name__ == '__main__':
	main()
	
```
#### 四、word2vec模型的使用
训练完成之后，我们可以利用训练好的模型来做一些词的预测，主要包括三个方面的应用。

##### 1、找出与指定词相似的词
返回的结果是一个列表，列表中包含了制定个数的元组，每个元组的键是词，值这个词语指定词的相似度。
##### 2、计算两个词的相似度
##### 3、根据前三个词来类比
代码如下：

``` javascript
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''=================================================
@IDE    ：PyCharm
@Author ：LuckyHuibo
@Date   ：2019/8/5 14:12
@Desc   ：对wiki_extractor_case进行测试
    1、加载训练好的语料corpus
    model = models.Word2Vec.load("./data/zhwiki_news.word2vec")
    #这个zhwiki_news.word2vec是我训练好的，你可以直接Load你训练好的model
    2、wordcloud画图时出现乱码
    ——绘制词云，需要添加.ttf字体，否则会显示乱码的
    .ttf文件在windows里面直接搜索即可，复制到同级目录即可，我用windows搜到的ping.ttf（太大了，未上传）
=================================================='''

# 找出与指定词相似的词
# 返回的结果是一个列表，列表中包含了制定个数的元组，每个元组的键是词，值这个词语指定词的相似度。
import logging
from gensim import models
import numpy as np
import matplotlib.pyplot as plt
from wordcloud import WordCloud


def get_mask():
    '''
    获取一个圆形的mask
    '''
    x, y = np.ogrid[:300, :300]
    mask = (x - 150) ** 2 + (y - 150) ** 2 > 130 ** 2
    mask = 255 * mask.astype(int)
    return mask


def draw_word_cloud(word_cloud):
    '''
    绘制词云，需要添加.ttf字体，否则会显示乱码的
    .ttf文件在windows里面直接搜索即可
    '''
    font = r'./data/ping.ttf'  # 这个太大，需要自己找下
    wc = WordCloud(background_color="white", font_path=font, mask=get_mask())
    wc.generate_from_frequencies(word_cloud)
    # 隐藏x轴和y轴
    plt.axis("off")
    plt.imshow(wc, interpolation="bilinear")
    plt.show()


def test_draw():
    '''
    测试绘制的词云
    :return:
    '''
    logging.basicConfig(format="%(asctime)s:%(levelname)s:%(message)s", level=logging.INFO)
    model = models.Word2Vec.load("./data/zhwiki_news.word2vec")  # 这个zhwiki_news.word2vec是我训练好的，你可以直接Load你训练好的model
    # 输入一个词找出相似的前10个词
    one_corpus = ["心理"]
    result = model.wv.most_similar(one_corpus[0], topn=100)
    # 将返回的结果转换为字典,便于绘制词云
    word_cloud = dict()
    for sim in result:
        # print(sim[0],":",sim[1])
        word_cloud[sim[0]] = sim[1]
    # 绘制词云
    draw_word_cloud(word_cloud)

    # #输入两个词计算相似度
    two_corpus = ["腾讯", "阿里巴巴"]
    res = model.wv.most_similar(two_corpus[0], two_corpus[1])
    print("similarity:", res)

    # 输入三个词类比
    three_corpus = ["北京", "上海", "广州"]
    res = model.wv.most_similar([three_corpus[0], three_corpus[1], three_corpus[2]], topn=100)
    # 将返回的结果转换为字典,便于绘制词云
    word_cloud = dict()
    for sim in res:
        # print(sim[0],":",sim[1])
        word_cloud[sim[0]] = sim[1]
    # 绘制词云
    draw_word_cloud(word_cloud)


if __name__ == "__main__":
    test_draw()

```
![enter description here](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1564987225443.png)
![enter description here](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1564987239131.png)

【Me】https://github.com/Valuebai/

【参考】
1、使用中文维基百科语料库训练一个word2vec模型：https://blog.csdn.net/sinat_29957455/article/details/81432846
——主要根据这个写的，更加具体清楚