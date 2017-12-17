---
title: 使用pandas做数据分析
date: 2017-10-03 09:01:58
tags: [pandas,python,pip]
categories: 数据分析
---

#### 安装:
```shell
$ pip install pandas
```
安装完成，系统中会包含三个包:numpy, pytz, pandas,主要使用的pandas,numpy 是一个扩展的数组

#### 引入类库:
```python
import pandas as pd
import numpy as np
from pandas import Series, DataFrame
```

#### Series 一个hash索引的数组，比普通的dict，map 计算更方便快捷:
```python
#初始化
ser = Series([6,5,4,9,2],index=['A','B','C','D','E'])
```
将得到一个以A,B,C,D,E为键，值为6,5,4,9,2的数组

##### 计算
```python
nser = ser[ser < 5] #将得到小于5的数组
nser = nser + 10 #将所有的值都添加10
nser = nser < 10 #转成boolean
'A' in nser #判断包含关系
A = Series([1,2,3,4])
B = Series([2,2,3,4])
C = A + B #相加
```

#### 二维检索计算的基本单位DataFrame

##### 数据加载构成DataFrame
```python
df=pd.read_csv("/Users/vbabu/Documents/personal/MyGitWork/csv/mls-salaries-2016.csv")
df=pd.read_json("/Users/vbabu/Documents/personal/MyGitWork/csv/mls-salaries-2016.json")
```

##### 获取头，尾的数据:
```python
df.head(count)
df.tail(count)
df.info() #获取DataFrame的相关存储设置
df['index_name'].head(count) # 获取某一列的头数据
df['new_name'] = df[new_name + 10] #添加新列，并且赋值
del df['name'] #删除列
df.fillna('5') #填充null的数据

```

##### 简单计算:
```python
df.sum(axis=1)
df.sum(axis=0)
df.min(axis=0) #求最小值
df.max(axis=0) #求最小值
df.groupby(['name']).count().sort_values().head() #相当于graylog里边的quick values
```

##### 数据删减
```python
dframe.drop('B') #删除数据
dframe.append([]) #添加数据
df = dframe[ 'B' > 10] #数据筛选
```
##### 数据排序:
```python
df.sort_values(ascending=True|False) 
```

##### 数据类型转换:
```python

df['col2'] = df['col2'].astype('int')
print '-----------'
print df.dtypes

df['col2'] = df['col2'].astype('float64')
print '-----------'
print df.dtypes

```

[数据翻译来源https://kanoki.org/2017/07/16/pandas-in-a-nutshell/#Selecting-Index](https://kanoki.org/2017/07/16/pandas-in-a-nutshell/#Selecting-Index)
