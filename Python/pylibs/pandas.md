
# 1.创建、读取、写入
- `pandas` 具有两种数据类型: `DataFrame` 和`Series`

## 1.1|DataFrame
- `DataFrame` 是一种二维表数据类型
- 可以使用`DataFrame()` 来创建一个`DataFrame` 对象：
- 对于传入的字典，每个键值对中的键被视为列名，值被视为列数据。
- 可以在构造函数中指定index来分配行名称

```python
# key corresponding to column name
# value corresponding to a column
>>> pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'],
...               'Sue': ['Pretty good.', 'Bland.']},
...                index=['Product A', 'Product B'])

                     Bob           Sue
Product A    I liked it.  Pretty good.
Product B  It was awful.        Bland.
```



## 1.2|Series
- `Series` 是一种单列数据类型
- `Series` 相当于`DataFrame` 的一行
- 可以用`Series()` 构造函数来创建`Series`对象
- 可以传入一个可迭代对象，并设定其`index` 和`Series` 的名字

```python
>>> pd.Series([30, 35, 40], index = ['2015 Sales', '2016 Sales', '2017 Sales'], name = 'Product A')
2015 Sales    30
2016 Sales    35
2017 Sales    40
Name: Product A, dtype: int64
```

## 1.3|读取csv及初探

- 通过`pandas.read_csv()`从`.csv` 文件中读取数据
```python
import pandas

# PATH = '...'
data = pands.read_csv('PATH')				#数据包含第一列，第一行为列属性，Header默认为0，index_col默认为None
data = pandas.read_csv('PATH',index_col=0)	#即把第一列作为索引，第一行为行属性
data = pandas.read_csv('PATH',header=None)	#第一行与第一列均为数据
```

- `DataFrame().shape` 属性显示了`DataFrame` 的形状大小 
```python
data.shape
```

-  `DataFrame().head(rows = 5 : int)` 可以返回DF前几行，默认5行
-  `DataFrame().tail(rows = 5 : int)` 可以返回DF末尾几行，默认5行

## 1.4|转存csv

将DataFrame存为csv格式:
```python
df.to_csv(path,sep=str,na_rep=str,float_format='%.',header=bool,index=bool)
'''
path			: 设定转存地址,可以为相对路径也可以是绝对路径,要带上后缀.csv .
sep				: 设定分隔符，默认逗号
na_rep			: 设定空值替换，默认''
float_format	: 设定小数格式,如'%.2f'代表两位小数
header			: 设定是否写入列标签，默认True
index			: 设定是否写入索引,默认True
'''
```

# 2.索引与选择

## 2.1|通过属性访问列


- `DataFrame` 为每一列创建了一个`Series`，可以通过`DataFrame.COLUMN_NAME`直接访问该`Series`
```python
>>> reviews
                     Bob           Sue
Product A    I liked it.  Pretty good.
Product B  It was awful.        Bland.
# Bob 是 reviews 中的一列
>>> reviews.Bob
Product A      I liked it.
Product B    It was awful.
Name: Bob, dtype: object
>>> type(reviews.Bob)
<class 'pandas.core.series.Series'>
```

- 根据`DataFrame` 作为一种类，可以使用`.get(Attribute_Name)` 来获取属性，这就允许方位内部具有空格的列:
```python
>>> test = pd.DataFrame({"A A": [1, 2], "B": [3, 4]})
>>> test
   A A  B
0    1  3
1    2  4
>>> test.get("A A")
0    1
1    2
Name: A A, dtype: int64
```

## 2.2|通过键值访问列

- 和字典相似，可以用`[]` 来访问列
```python
>>> reviews["Bob"]
Product A      I liked it.
Product B    It was awful.
Name: Bob, dtype: object
>>> type(reviews["Bob"])
<class 'pandas.core.series.Series'>
```

- 相比于属性，直接使用`[]` 来访问列更快，也适用于内部具有空格字符或不合法变量名的数
```python
>>> test = pd.DataFrame({"A A": [1, 2], "B": [3, 4], 22 : [5, 6]})
>>> test
   A A  B  22
0    1  3   5
1    2  4   6
>>> test.get(22)
0    5
1    6
Name: 22, dtype: int64
>>> test[22]
0    5
1    6
Name: 22, dtype: int64
>>> test.22
  File "<stdin>", line 1
    test.22
          ^
SyntaxError: invalid syntax
```

## 2.3|iloc和loc

- `iloc` 为按索引顺序取DataFrame中值
- `.iloc[ROWS_EXPRESSION, COLS_EXPRESSION]` 为使用分片技术等取值
  - `EXPRESSION` 支持: **单个值、分片如`0:40:5`, 及一系列值`[1, 3, 5, 7, 12]**`
```python
>>> test
   A   B   C
0  0   0   0
1  1   2   3
2  2   4   6
3  3   6   9
4  4   8  12
5  5  10  15
6  6  12  18
7  7  14  21
>>> test.iloc[[0, 4, 7], -2:]
    B   C
0   0   0
4   8  12
7  14  21
```


- `loc` 为按标签取值
- `.loc[ROWS_EXPRESSION, COLS_EXPRESSION]` 支持: **单个值、分片如`0:40:5`, 及一系列值`[1, 3, 5, 7, 12]**`
- 值得注意的是`.loc` 中的索引是包含头尾的:
```python
In [3]: test
Out[3]:
   A   B   C
0  0   0   0
1  1   2   3
2  2   4   6
3  3   6   9
4  4   8  12
5  5  10  15
6  6  12  18
7  7  14  21
8  8  16  24
9  9  18  27
In [6]: test.loc[0:5, "A":"B"]
Out[6]:
   A   B
0  0   0
1  1   2
2  2   4
3  3   6
4  4   8
5  5  10
```


## 2.4|设置索引列

- 使用`DataFrame.set_index("COLUMN_NAME")` 可以设置索引列，也可以通过具有相同长度的可迭代对象作为索引列
```python
In [16]: test
Out[16]:
   A  B   C
0  0  0   0
1  1  2   3
2  2  4   6
3  3  6   9
4  4  8  12

In [17]: test.set_index("B")
Out[17]:
   A   C
B
0  0   0
2  1   3
4  2   6
6  3   9
8  4  12
```
- 此操作是不可逆的，要想回到默认索引，则需新建一个索引列或使用。

- 可以使用多索引

## 2.5|条件索引

- 表达式 `df.loc[ expression(df.column1, df.column), ... ]`
- `expression` 的作用是返回一一个布尔类型列表，表中值为`expression` 是否正确
- 由此可见，`df.loc` 可以接受一布尔列表作为是否取改行的指示
- 当使用逻辑符号时，应使用`&`  `|` 等位运算符号， 而非`and` 等逻辑符号
```python
In [22]: test
Out[22]:
   A  B   C
0  0  0   0
1  1  2   3
2  2  4   6
3  3  6   9
4  4  8  12

In [23]: test.loc[test.get("A") >= 3, :]
Out[23]:
   A  B   C
3  3  6   9
4  4  8  12

In [25]: test.A >= 3
Out[25]:
0    False
1    False
2    False
3     True
4     True
Name: A, dtype: bool

# 直接使用布尔列表
In [26]: test.loc[[False, False, False, True, True], :]
Out[26]:
   A  B   C
3  3  6   9
4  4  8  12
```


这和mysql是很相似的:
```mysql
select 
	*
from
	df
where
	df.column1 ...
	df.column2 ...;
```

- 还有一些可以用来做条件筛选的函数
- `pandas.Series.isin(Iterable)`  相当于mysql中的`IN`一对多匹配
- `pandas.Series.isnull(Iterable)` 相当于mysql中的`ISNULL` 空值匹配
- `pandas.Series.notnull(Iterable)` 相当于mysql中的`NOT NULL` 非空值匹配


## 2.6|列赋值

- 可以对列进行广播赋值，即一列均赋一个值
- 也可以对列进行等幂赋值
```python
In [29]: test
Out[29]:
   A  B   C
0  0  0   0
1  1  2   3
2  2  4   6
3  3  6   9
4  4  8  12

# 广播赋值
In [30]: test.A = 20

In [31]: test
Out[31]:
    A  B   C
0  20  0   0
1  20  2   3
2  20  4   6
3  20  6   9
4  20  8  12

In [36]: test.A = [i**2 for i in range(5)]

In [37]: test
Out[37]:
    A  B   C
0   0  0   0
1   1  2   3
2   4  4   6
3   9  6   9
4  16  8  12
```


# 3.聚合函数和映射

## 3.1|聚合函数


- `describe()` 函数提供数值序列的统计信息，包括计数信息，均值等统计学信息
- `describe()` 函数提供字符串序列的统计信息，包括数量、频次、独特数量等统计信息
```python
>>> reviews.country.describe()
count     150925
unique        48
top           US
freq       62397
Name: country, dtype: object
>>> reviews.points.describe()  
count    150930.000000      
mean         87.888418      
std           3.222392      
min          80.000000      
25%          86.000000      
50%          88.000000      
75%          90.000000      
max         100.000000      
Name: points, dtype: float64
```


- `pandas.Series.describe` 可以显示某一列的统计信息
- `pandas.DataFrame.describe` 可以显示整个表的统计信息， 如果所有字段都为字符串，则统计字符串信息，一旦存在数值字段则只按数值方式统计信息
```python
# 无数值字段
>>> reviews.loc[:, ("country", "designation", "province")].describe()
       country designation    province
count   150925      105195      150925
unique      48       30621         455
top         US     Reserve  California
freq     62397        2752       44508

# 有数值字段
>>> reviews.loc[:, ("price","country", "designation", "province")].describe() 
               price
count  137235.000000
mean       33.131482
std        36.322536
min         4.000000
25%        16.000000
50%        24.000000
75%        40.000000
max      2300.000000
```

- 最常见的还包括:
- `Series.mean` 求均值函数，和sql 的`MEAN` 差不多
- `Series.median` 求中位数函数 
- `Series.unique` 求不重合值，和sql 的`UNIQUE` 或`DISTINCT` 差不多
- `Series.sum` 总和函数， 和SQL 的 `SUM` 差不多
- `Series.value_counts()` 统计函数，作用于统计各种值出现的频次，可以作用在数值和字符串字段


```python
>>> reviews.country.unique()
array(['US', 'Spain', 'France', 'Italy', 'New Zealand', 'Bulgaria',
       'Argentina', 'Australia', 'Portugal', 'Israel', 'South Africa',
       'Greece', 'Chile', 'Morocco', 'Romania', 'Germany', 'Canada',
       'Moldova', 'Hungary', 'Austria', 'Croatia', 'Slovenia', nan,
       'India', 'Turkey', 'Macedonia', 'Lebanon', 'Serbia', 'Uruguay',
       'Switzerland', 'Albania', 'Bosnia and Herzegovina', 'Brazil',
       'Cyprus', 'Lithuania', 'Japan', 'China', 'South Korea', 'Ukraine',
       'England', 'Mexico', 'Georgia', 'Montenegro', 'Luxembourg',
       'Slovakia', 'Czech Republic', 'Egypt', 'Tunisia', 'US-France'],
      dtype=object)
>>> reviews.price.value_counts()
20.0     7860
15.0     7056
18.0     5988
25.0     5955
30.0     5449
         ...
612.0       1
271.0       1
448.0       1
292.0       1
217.0       1
Name: price, Length: 357, dtype: int64
>>> reviews.country.value_counts()
US                        62397
Italy                     23478
France                    21098
Spain                      8268
Chile                      5816
...
US-France                     1
Name: country, dtype: int64
```

## 3.2|映射

### 3.2.1|map

- `pandas.Series.map` 可以理解为函数，它接受一个函数作为参数，并用当前Series作为输入计算输出:
- `s.map(lambda x : x * x)` 表示返回 `s` 中每个元素的平方
```python
>>> test
   A  B   C
0  0  0   0
1  1  2   3
2  2  4   6
3  3  6   9
4  4  8  12
>>> test["A"].map(lambda x : x*x)
0     0
1     1
2     4
3     9
4    16
Name: A, dtype: int64
```

### 3.2.2|apply

```python
DataFrame.apply(func, axis=0, raw=False, result_type=None, args=(), **kwargs)
```
- 与`map` 不同，`apply`可以将函数作用于表中的某一个维度(由axis控制)
- `func` 为传参，
  - 当返回一个值时，apply的结果是`Series`
  - 当返回可迭代对象时, apply的结果是 `DataFrame`
- `axis = {0 or "index", 1 or "columns"}` 表示计算的维度，
  - index时 平行于index列计算， 所以传入是列
  - columns 平行于columns行计算，所以传入是行，函数也可以使用columns中的各字段
- `args` 和 `**kwargs`  为对func的传参

```python
>>> test
   A  B   C
0  0  0   0
1  1  2   3
2  2  4   6
3  3  6   9
4  4  8  12
>>> test.apply(lambda x: sum(x)) #默认是index，即每列计算一个结果
A    10
B    20
C    30
dtype: int64
>>> test.apply(lambda x: (x.sum(), x.mean())) # 当func的返回值是多个值
      A     B     C
0  10.0  20.0  30.0
1   2.0   4.0   6.0
>>> test.apply(lambda x: x.A * x.B, axis = "columns") # 是Columns时传入为行
0     0
1     2
2     8
3    18
4    32
```

## 3.2.3|符号运算

- 列支持使用二元运算符快速计算值
- 这些包括 `+, -, *, /, ==, `

```python
>>> test
   A  B  D  E
0  0  0  r  a
1  1  2  e  p
2  2  4  p  p
3  3  6  l  l
4  4  8  y  y
>>> test.D == test.E
0    False
1    False
2     True
3     True
4     True
dtype: bool
>>> test.D + test.E
0    ra
1    ep
2    pp
3    ll
4    yy
dtype: object
>>> test.A + test.B
0     0
1     3
2     6
3     9
4    12
dtype: int64
```


# archive
## DataFrame 相关
### 读取数据


- 通过`pandas.DataFrame.from_dict(data, orient, dtype, columns)`从字典中读取数据，详见[官网](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.from_dict.html). 
- 以下是从单一字典中读取数据的示例
1. 默认情况下，字典的`键值`是数据的column，`orient` 参数默认的值是`columns`
```python
>>> data = {'col_1': [3, 2, 1, 0], 'col_2': ['a', 'b', 'c', 'd']}
>>> pd.DataFrame.from_dict(data)
   col_1 col_2
0      3     a
1      2     b
2      1     c
3      0     d
```
2. 修改`orient`为`index` 时， 字典的键值用作数据的index
```python
>>> data = {'col_1': [3, 2, 1, 0], 'col_2': ['a', 'b', 'c', 'd']}
>>> pandas.DataFrame.from_dict(data, orient = 'index')
       0  1  2  3
col_1  3  2  1  0
col_2  a  b  c  d
```
当`orient`为`index` 时，可以为设置columns
```python
>>> data = {'col_1': [3, 2, 1, 0], 'col_2': ['a', 'b', 'c', 'd']}
>>> pandas.DataFrame.from_dict(data, orient='index', columns = ['A', 'B', 'C', 'D'])
       A  B  C  D
col_1  3  2  1  0
col_2  a  b  c  d
```


### 数据操作
- `pandas.DataFrame.apply(function, axis=0, raw=False)`可以按column(`axis=0`)或按index(`axis=1`)对DataFrame应用函数
```python
>>> df = pandas.DataFrame([[4, 9]] * 3, columns=['A', 'B'])
>>> df
   A  B
0  4  9
1  4  9
2  4  9
>>> df.apply(numpy.sqrt)
     A    B
0  2.0  3.0
1  2.0  3.0
2  2.0  3.0
```
### 数据处理

### 存取

使用pandas转存csv文件

```python
import pandas as pd

# PATH = '...'
data = pd.read_csv('PATH')				#数据包含第一列，第一行为列属性，Header默认为0，index_col默认为None
data = pd.read_csv('PATH',index_col=0)	#即把第一列作为索引，第一行为行属性
data = pd.read_csv('PATH',header=None)	#第一行与第一列均为数据
```

检查行、列信息

```python
data.index								#二维数据行属性
data.columns							#二维数据列属性
```

数据抽取  

```python
data.loc['index','column']				#按照行列名进行抽取，可以使用切片
data.iloc[i,j]							#按照行列序号进行抽取，可以使用切片
```

使用函数将字典转为DataFrame：

```python
df = pd.DataFrame.from_dict([dict,])	#将列表中的一系列字典转为DataFrame
```

将DataFrame存为csv格式:

```python
df.to_csv(path,sep=str,na_rep=str,float_format='%.',header=bool,index=bool)
'''
path			: 设定转存地址,可以为相对路径也可以是绝对路径,要带上后缀.csv .
sep				: 设定分隔符，默认逗号
na_rep			: 设定空值替换，默认''
float_format	: 设定小数格式,如'%.2f'代表两位小数
header			: 设定是否写入列标签，默认True
index			: 设定是否写入索引,默认True
'''
```



### 预处理

使用pandas查看文件概要

```python
data.head()
```

时间处理

```python
import datetime

time = "10/12/2019 17:57:50"
d = datetime.datetime.strptime(time,"%Y-%m-%d %H:%M:%S")
print(d)
---------------------------------
datetime.datetime(2019, 10, 12, 17, 57, 50)
```

  



### 数据操作

求差分

```
series = [...]
delta = [series[i+1]-series[i] for i in range(len(series)-1)]
```

求平滑

``` python
import numpy as np

Series = []								#Series为被求平滑对象
N = N									#N为平滑窗长度
mode='MODE'								#MODE有'full','same','valid'三个选择，将导致不同长度
np.convolve(Series, np.ones((N,))/N, mode='MODE')
```

使用`pands.DataFrame.apply()`按columns 遍历
```python
import 
```
