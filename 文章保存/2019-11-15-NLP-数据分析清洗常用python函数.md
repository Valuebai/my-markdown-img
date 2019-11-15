---
title: 2019-11-15-NLP-数据分析清洗常用python函数
tags: python,
author:  Valuebai
---


```python
# 业内习惯性将加载数据用函数def load_data()来处理
# 加载数据，并将处理的数据存到pureContent.csv中

# path_root: 文件的路径
# filename: 默认名字为pureContent.csv
def load_data(path_root, filename='pureContent.csv'):
    pure_file = os.path.join(path_root, filename)
	
	# 判断是否存在处理过pure_file
    if not os.path.exists(pure_file):
	    print('File not exist! processing...')
        news_file = os.path.join(path_root, 'sqlResult_1558435.csv')	# 要处理的原始文件
        news_content = pd.read_csv(news_file, encoding='gb18030')
        #fillna()会将DataFrame中nan数据的数据填充为想要的数据，并返回填充后的结果。这里讲NANt填充为空
        news_content['content'] = news_content['content'].fillna('')
        pure_content = pd.DataFrame()
        pure_content['content'] = news_content['content']
        pure_content = pure_content.fillna('')
        pure_content['tokenized_content'] = pure_content['content'].apply(cut)
        pure_content.to_csv(pure_file, encoding='gb18030')
    else:
        print('File found! ')
        pure_content = pd.read_csv(pure_file, encoding='gb18030')
        pure_content = pure_content.fillna('')
    return pure_content
	
```





```python
# 数据清洗 - 去除空值
# 文本型字段空值改为“缺失数据”，数字型字段空值改为 0 

def data_cleaning(df):
    cols = df.columns
    for col in cols:
        if df[col].dtype ==  'object':
            df[col].fillna('缺失数据', inplace = True)
        else:
            df[col].fillna(0, inplace = True)
    return(df)



---
# 该函数可以将任意数据内空值替换

data_c1 = data_cleaning(data)
print(data_c1.head(10))
```








```python
# 数据清洗 - 时间标签转化
# 将时间字段改为时间标签

def data_time(df,*cols):
    for col in cols:
        df[col] = df[col].str.replace('年','.')
        df[col] = df[col].str.replace('月','.')
        df[col] = df[col].str.replace('日','')
        df[col] = pd.to_datetime(df[col])
    return(df)
	
	
---
# 该函数将输入列名的列，改为DatetimeIndex格式

data_c2 = data_time(data_c1,'数据获取日期')
print(data_c2.head(10))
```





【Me】https://github.com/Valuebai/

【参考】
1、出处：地址