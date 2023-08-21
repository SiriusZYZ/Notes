
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

### 3.1.1|describe

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
- `Series.mean` 求均值函数，只能作用在数值字段中
- `Series.unique` 求不重合值，和sql 的`UNIQUE` 或`DISTINCT` 差不多
- `Series.sum` 总和函数
