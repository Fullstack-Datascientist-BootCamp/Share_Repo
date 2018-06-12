> 暴露，是为了更好的插入～

### Data Structure
---
OOP是非常优秀的编程思想，那在学习pandas时，我们重点研究两种非常重要的对象，Series和DataFrame，Series是一列数据，DataFrame是很多列数据在一起。那我们先来看Series。

#### Series
---
Series 是**一维**的能够接受**任何数据形式**（整数，字符串，浮点数，python对象等）的结构，通常我们用
```python
import pandas as pd
s = pd.Series(data, index=index)
```
这种方式来新建一个Series对象, 举三个例子
```python
import numpy as np
s = pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])

d = {'a': 4, 'c': 2, 'b': 3}
s = pd.Series(d)

s = pd.Series(5, index=['a', 'b', 'c', 'd', 'e'])
```
---
这样，我们就得到了一个Series对象实例，那这个对象都有哪些方法呢？
Seires对象有两个相似的数据结构，**ndarray**和**dict**,我们可以用一些类似的方法
```python
# ndarray
s[0]  # 索引
s[:3]  # 范围
s[s > s.median]  # 判断
s[[4, 3, 1]]  # 列表索引
np.exp(s)  # e的指数幂

# dict
s['a']i
s.get('a')
s['e'] = 12
s.get('f', 3)
```
另外值得注意的是，Series还有一个单独看没什么用的name属性
```python
s = pd.Series(data, index=index, name='something')
s.name
```
---
#### DataFrame
---
DataFrame 是一个**二维**标签的数据结构，我们可以把它看成一张sheet表、SQL表或是一个Series的字典，它是pandas最经常被使用的对象。和Series一样，我们先获得一个DataFrame对象（这里只介绍简单的两种，[查看更多](https://pandas.pydata.org/pandas-docs/stable/dsintro.html#DataFrame)）
```python
df = pd.DataFrame({'A': pd.Series(np.random.randn(10)),
                  'B': pd.Series(np.random.randn(11))},
                  index=range(11))

df = pd.read_csv('sample.csv',
                 names=['uid', 'similarquid', 'preansname', 'sufansname'],
                 index=pd.date_range('20180601', periods=100))
```
同样，当我们有了DataFrame对象之后，有哪些方法？
```python
#简单获取一些表的信息
print(df)
df.info()
df.dtypes

df.head()
df.tail(10)

df.index
df.columns
df.values
df.describe()

df.sort_index(axis=1, ascending=False)
df.sort_values(by='B')

#选中一张表中的某些行、列, 甚至一个“单元格”
#简单常用的get方法
df['A']
df[0:3]
df['20180603':'20180607']

#推荐在生产中使用的方法 loc, iloc, at, iat
df.loc[row_indexer, column_indexer]  # 通过标签去定位行、列
# 值得注意的是，loc可以只定位行，所以列的参数是optional的
df.iloc[int(row), int(column)]  # 通过位置(index)去定位行、列
df.at['20180601', 'A']
df.iat[0, 0]

#布尔检索
df[df['A'] == 3]
df[df['A'].isin(['2014', '2016'])]
```
在看完了DataFrame的基础操作方法之后，不妨停下来想一想，**排序**、**筛选**、**搜索**、**修改**我们都已经做到了，这就是数据清洗的一些工作。那作为数据分析的王牌之包，pandas在数据处理上当然有为人称道的本领，需要注意的是，pandas的方法往往有很多参数用来调优，希望大家在初学的时候能够多去查阅[Document](https://pandas.pydata.org/pandas-docs/stable/index.html)。

        `df.mean()`：求平均值   `df.describe()`：获取常用统计数据
        `df.apply(fuc)`：对data运行一个函数
        `s.value_counts()`：相当于python中`collections.Counter()`
        `pd.concat(df1, df2, df3)`：能够连接几个DataFrame
        `pd.merge(df1, df2, on='key')`：相当于SQL表的join
        `df.append()`：能够用于增加行
        `df.groupby('A').count()` or `df.groupby(['A', 'B']).sum()`：相当于split，apply，combine（aggregate）
        `df.to_csv('sample.csv', index=False)`：把处理后的DataFrame写成文件


*文中内容完全来源于官方文档[10 minutes to pandas](https://pandas.pydata.org/pandas-docs/stable/10min.html)，如有错误，恳请指正～
