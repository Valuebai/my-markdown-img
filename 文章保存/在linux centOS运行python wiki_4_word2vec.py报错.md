---
title: 在linux centOS运行python wiki_4_word2vec.py报错
tags: python,
author:  [Valuebai](https://github.com/Valuebai/)
---
1.报错信息

``` stata
[root@vultr ~/learn-NLP-luhuibo/lesson-04]# python wiki_4_word2vec.py 
Traceback (most recent call last):
  File "wiki_4_word2vec.py", line 19, in <module>
    model = Word2Vec(LineSentence(infile), size=400, window=5, min_count=5, workers=multiprocessing.cpu_count())
  File "/usr/local/lib/python3.7/site-packages/gensim/models/word2vec.py", line 783, in __init__
    fast_version=FAST_VERSION)
  File "/usr/local/lib/python3.7/site-packages/gensim/models/base_any2vec.py", line 759, in __init__
    self.build_vocab(sentences=sentences, corpus_file=corpus_file, trim_rule=trim_rule)
  File "/usr/local/lib/python3.7/site-packages/gensim/models/base_any2vec.py", line 943, in build_vocab
    self.trainables.prepare_weights(self.hs, self.negative, self.wv, update=update, vocabulary=self.vocabulary)
  File "/usr/local/lib/python3.7/site-packages/gensim/models/word2vec.py", line 1876, in prepare_weights
    self.reset_weights(hs, negative, wv)
  File "/usr/local/lib/python3.7/site-packages/gensim/models/word2vec.py", line 1889, in reset_weights
    wv.vectors = empty((len(wv.vocab), wv.vector_size), dtype=REAL)
numpy.core._exceptions.MemoryError: Unable to allocate array with shape (837709, 400) and data type float32
```


2.wiki_4_word2vec.py代码：
```
import multiprocessing
from gensim.models import Word2Vec
from gensim.models.word2vec import LineSentence

if __name__ == "__main__":
    infile = './data/wiki-jieba-zh-words.txt'
    outp1 = './data/wiki-zh-model'
    outp2 = './data/wiki-zh-vector'

    model = Word2Vec(LineSentence(infile), size=400, window=5, min_count=5, workers=multiprocessing.cpu_count())
    model.save(outp1)
    model.save_word2vec_format(outp2, binary=False)
```

3. 在linux-centOS上pip install multiprocessing报错
在linux上安装了python3，原来的是python2
```
[root@vultr ~/learn-NLP-luhuibo/lesson-04]# pip install multiprocessing
Collecting multiprocessing
  Using cached https://files.pythonhosted.org/packages/b8/8a/38187040f36cec8f98968502992dca9b00cc5e88553e01884ba29cbe6aac/multiprocessing-2.6.2.1.tar.gz
    ERROR: Command errored out with exit status 1:
     command: /usr/local/bin/python3.7 -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-aarp_c33/multiprocessing/setup.py'"'"'; __file__='"'"'/tmp/pip-install-aarp_c33/multiprocessing/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' egg_info --egg-base pip-egg-info
         cwd: /tmp/pip-install-aarp_c33/multiprocessing/
    Complete output (6 lines):
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/tmp/pip-install-aarp_c33/multiprocessing/setup.py", line 94
        print 'Macros:'
                      ^
    SyntaxError: Missing parentheses in call to 'print'. Did you mean print('Macros:')?
    ----------------------------------------
ERROR: Command errored out with exit status 1: python setup.py egg_info Check the logs for full command output.

```
