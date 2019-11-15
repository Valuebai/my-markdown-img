---
title: 2019-11-15-NLP-数据分析清洗常用python函数
tags: python,
author:  Valuebai
---



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