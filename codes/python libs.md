# 标准库

## random随机功能库

处理随机数相关,导入方法:

```python
import random
```

形成0-1之间随机浮点数

```python
random.random()
```

形成[a,b]之间的随机__浮点数__

```python
random.uniform(a,b)
```

形成[a,b]之间的随机__整数__

```python
random.randint(a,b)
```

从元祖、列表中选取一数

```python
random.choice(LIST)
```

从元祖、列表中随机选出__n__个数

```python
random.sample(LIST,n)
```

随机打乱列表的顺序

```python
random.shuffle(LIST)
```

## time时间功能库

### 处理器时间

==**获取当前处理器时间**==
```python
time.process_time()
```
- `process_time()`此函数返回自第一次调用此函数以来经过的 wallclock 秒数，作为浮点数

### 纪元秒数
- *`epoch`* 是自从`1970-01-01 00:00:00 (UTC)` 以来的描述

==**获取纪元秒数**==
```python
time.time()
```
- `time()` 返回当前纪元时间，且不接受任何参数

==**从纪元秒数转换到历法时间**==
```python
# epoch time -> GMT time_struct
time.gmtime(sec)

# epoch time -> local time time_struct
time.localtime(sec)
```
- `gmtime(epoch)` 将纪元秒数转换为GMT时间的结构体`time.struct_time`，当无参调用时返回当前纪元秒数的转换结果
- `localtime(epoch)` 将纪元秒数转换为本地时间的结构体`time.struct_time`，当无参调用时返回当前纪元秒数的转换结果

==**从本地历法时间结构体转换为纪元秒数**==
```python
# local time time_struct -> epoch time
time.mktime(ts)
```
- `mktime(ts)`将本地时间结构体转换为纪元秒数，函数接收一个参数

### 历法时间

==**获取当前时间结构体**==
```python
# GMT time_struct
time.gmtime()

# local time time_struct
time.localtime()
```
- `gmtime()` 获取当前GMT时间的结构体`time.struct_time`
- `localtime()` 获取当前本地时间的结构体`time.struct_time`

### 时间的输入和导出

==**转义字符串**==
- 在`format`中*常用* 的关键字如下:

| 符号 | 含义                         |
| ---- | ---------------------------- |
| `%y` | 两位数的年                   |
| `%Y` | 带世纪的年                   |
| `%m` | 月份                         |
| `%d` | 日                           |
| `%H` | 小时(24小时制)               |
| `%I` | 小时(12小时制)               |
| `%p` | 上午或下午                   |
| `%M` | 分钟                         |
| `%S` | 秒                           |
| `%a` | 星期的缩写                   |
| `%A` | 星期的完整写法               |
| `%b`| 月份的缩写|
| `%B` | 月份的完整写法 |
| `%x` | 本地化的日期表达 = `%m/%d/y` |
| `%X` | 本地化的时间表达`%H:%M:%S`   |
| `%%`     | 字面上的`%`                              |

==**将字符串转换为时间结构体**==
```python
time.strptime(string, format)
```
- `strptime(string, format)` 将字符串`string`按`format`格式提取时间值并返回`struct_time`

**==将时间结构体转换为特定格式字符串==**
```python
time.strftime(format, time_struct)
```
- `strftime(format, time_struct)` 将时间结构体转换为特定的字符串[参考](https://docs.python.org/zh-cn/3.7/library/time.html)
- `time_struct`参数缺省时默认为`localtime()`返回的值




# DataFrame 相关
## 读取数据


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


## 数据操作
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
# 数据处理

## 存取

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



## 预处理

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

  



## 数据操作

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

# 利用matplotlib可视化

## plt使用

### 简单例子

```  python
import matplotlib.pyplot as plt

plt.figure(figsize=(length,heigth))		#设置图片大小
plt.plot(x,y)							#折线图绘制x-y数据
#同一个plot下只要不进行plt.imshow()就可加很多的线
plt.legend()							#设置图例
plt.grid()								#设置网格
```

图例设置:

``` python
plt.legend([Legend,..],bbox_to_anchor=(1.05,0), loc=3, borderaxespad=0)	#控制图例位置
```

### 绘图元素控制

- 坐标轴控制:  

``` python
#范围控制
ax.set_xlim(,)			#上限不一定要比下限大
ax.set_ylim()

#刻度控制
ax.set_xticks([,],[,])	#首个参数为刻度值，第二个参数为刻度值对应的文本，可缺省。
ax.set_yticks([,],[,])

#对数轴控制
ax.set_xscale('log')
ax.set_yscale('')
```





### 绘制多图

```python
fig = plt.figure(figsize=(length,height))

#设置总标题
plt.suptitle('')

#申请子图
ax = fig.add_subplot(nrows, ncols, index, **kwargs)		
```

其中，nrows, ncols代表你想大图里面分多少行、列，index代表你的这个子图放在上述区块中的第几个

```python
ax = fig.add_subplot(3,3, 5, **kwargs)
```

| 1    | 2         | 3    |
| ---- | --------- | ---- |
| 4    | ***5*** | 6    |
| 7    | 8         | 9    |

tips:

在同一个大图里面每张子图的大小是由nrow, ncols决定的，如果需要不同大小的图，可以改变申请子图的函数中这两个参数。

例如：

```python
fig = plt.figure(figsize=(12,12))

#===============
#  subplot
#===============
ax = fig.add_subplot(2, 2, 1)

#===============
#   subplot
#===============
ax = fig.add_subplot(2, 2, 2)

#===============
#   subplot
#===============

ax = fig.add_subplot(2, 1, 2)

```

得到这样的图片:

![](笔记本.assets/multiple_subplot.png)

使用plt中相同的函数进行绘图:

```python
ax.plot()
ax.bar()
...
```

对于每个子图的绘图元素控制，可以利用ax对象的方法进行:

```python
#设置坐标轴范围
ax.set_xlim()
ax.set_ylim()

#设置坐标轴刻度
ax.set_xticks()
ax.set_yticls()

#设置坐标轴标题
ax.set_xlabel()
ax.set_ylabel()

#设置子图图题
ax.set_title()

#设置网格
ax.grid()

#设置图例
ax.legend()
```



### plot函数使用方式

``` python
plt.plot(y)								#缺省x数据，直接作图
plt.plot(x,y,color='COLOR',linestyle='STYLE',label='LABEL'linewidth=L)		#颜色、线形、标签,线宽的设置
```
### pyplot线形

``` python
'''
线形:
'-'       solid line style
'--'      dashed line style
'-.'      dash-dot line style
':'       dotted line style
'''
```

## matplotlib颜色

``` python
'''
颜色:
1:color=(R,G,B)
注意R,G,B三个值只能在0-1之间
2:color='COLOR':
cnames = {
'aliceblue':            '#F0F8FF',
'antiquewhite':         '#FAEBD7',
'aqua':                 '#00FFFF',
'aquamarine':           '#7FFFD4',
'azure':                '#F0FFFF',
'beige':                '#F5F5DC',
'bisque':               '#FFE4C4',
'black':                '#000000',
'blanchedalmond':       '#FFEBCD',
'blue':                 '#0000FF',
'blueviolet':           '#8A2BE2',
'brown':                '#A52A2A',
'burlywood':            '#DEB887',
'cadetblue':            '#5F9EA0',
'chartreuse':           '#7FFF00',
'chocolate':            '#D2691E',
'coral':                '#FF7F50',
'cornflowerblue':       '#6495ED',
'cornsilk':             '#FFF8DC',
'crimson':              '#DC143C',
'cyan':                 '#00FFFF',
'darkblue':             '#00008B',
'darkcyan':             '#008B8B',
'darkgoldenrod':        '#B8860B',
'darkgray':             '#A9A9A9',
'darkgreen':            '#006400',
'darkkhaki':            '#BDB76B',
'darkmagenta':          '#8B008B',
'darkolivegreen':       '#556B2F',
'darkorange':           '#FF8C00',
'darkorchid':           '#9932CC',
'darkred':              '#8B0000',
'darksalmon':           '#E9967A',
'darkseagreen':         '#8FBC8F',
'darkslateblue':        '#483D8B',
'darkslategray':        '#2F4F4F',
'darkturquoise':        '#00CED1',
'darkviolet':           '#9400D3',
'deeppink':             '#FF1493',
'deepskyblue':          '#00BFFF',
'dimgray':              '#696969',
'dodgerblue':           '#1E90FF',
'firebrick':            '#B22222',
'floralwhite':          '#FFFAF0',
'forestgreen':          '#228B22',
'fuchsia':              '#FF00FF',
'gainsboro':            '#DCDCDC',
'ghostwhite':           '#F8F8FF',
'gold':                 '#FFD700',
'goldenrod':            '#DAA520',
'gray':                 '#808080',
'green':                '#008000',
'greenyellow':          '#ADFF2F',
'honeydew':             '#F0FFF0',
'hotpink':              '#FF69B4',
'indianred':            '#CD5C5C',
'indigo':               '#4B0082',
'ivory':                '#FFFFF0',
'khaki':                '#F0E68C',
'lavender':             '#E6E6FA',
'lavenderblush':        '#FFF0F5',
'lawngreen':            '#7CFC00',
'lemonchiffon':         '#FFFACD',
'lightblue':            '#ADD8E6',
'lightcoral':           '#F08080',
'lightcyan':            '#E0FFFF',
'lightgoldenrodyellow': '#FAFAD2',
'lightgreen':           '#90EE90',
'lightgray':            '#D3D3D3',
'lightpink':            '#FFB6C1',
'lightsalmon':          '#FFA07A',
'lightseagreen':        '#20B2AA',
'lightskyblue':         '#87CEFA',
'lightslategray':       '#778899',
'lightsteelblue':       '#B0C4DE',
'lightyellow':          '#FFFFE0',
'lime':                 '#00FF00',
'limegreen':            '#32CD32',
'linen':                '#FAF0E6',
'magenta':              '#FF00FF',
'maroon':               '#800000',
'mediumaquamarine':     '#66CDAA',
'mediumblue':           '#0000CD',
'mediumorchid':         '#BA55D3',
'mediumpurple':         '#9370DB',
'mediumseagreen':       '#3CB371',
'mediumslateblue':      '#7B68EE',
'mediumspringgreen':    '#00FA9A',
'mediumturquoise':      '#48D1CC',
'mediumvioletred':      '#C71585',
'midnightblue':         '#191970',
'mintcream':            '#F5FFFA',
'mistyrose':            '#FFE4E1',
'moccasin':             '#FFE4B5',
'navajowhite':          '#FFDEAD',
'navy':                 '#000080',
'oldlace':              '#FDF5E6',
'olive':                '#808000',
'olivedrab':            '#6B8E23',
'orange':               '#FFA500',
'orangered':            '#FF4500',
'orchid':               '#DA70D6',
'palegoldenrod':        '#EEE8AA',
'palegreen':            '#98FB98',
'paleturquoise':        '#AFEEEE',
'palevioletred':        '#DB7093',
'papayawhip':           '#FFEFD5',
'peachpuff':            '#FFDAB9',
'peru':                 '#CD853F',
'pink':                 '#FFC0CB',
'plum':                 '#DDA0DD',
'powderblue':           '#B0E0E6',
'purple':               '#800080',
'red':                  '#FF0000',
'rosybrown':            '#BC8F8F',
'royalblue':            '#4169E1',
'saddlebrown':          '#8B4513',
'salmon':               '#FA8072',
'sandybrown':           '#FAA460',
'seagreen':             '#2E8B57',
'seashell':             '#FFF5EE',
'sienna':               '#A0522D',
'silver':               '#C0C0C0',
'skyblue':              '#87CEEB',
'slateblue':            '#6A5ACD',
'slategray':            '#708090',
'snow':                 '#FFFAFA',
'springgreen':          '#00FF7F',
'steelblue':            '#4682B4',
'tan':                  '#D2B48C',
'teal':                 '#008080',
'thistle':              '#D8BFD8',
'tomato':               '#FF6347',
'turquoise':            '#40E0D0',
'violet':               '#EE82EE',
'wheat':                '#F5DEB3',
'white':                '#FFFFFF',
'whitesmoke':           '#F5F5F5',
'yellow':               '#FFFF00',
'yellowgreen':          '#9ACD32'}
'''
```



## 利用matplotlib进行三维作图

### 三维曲线作图



```python
from mpl_toolkits.mplot3d import Axes3D			#导入3D作图模块

fig = plt.figure(figsize=())						#设定图大小
ax  = Axes3D(fig, auto_add_to_figure=False)		
fig.add_axes(ax)

ax.plot(X,Y,Z)									#以序列的X,Y,Z分量进行三维曲线作图

ax.view_init(elev, azim)						#设定视角，elev代表垂直视角，默认30度；azim代表水平视角，默认-60度。

#设定各轴范围
ax.set_xlim()
ax.set_ylim()
ax.set_zlim()

#设定各轴标签
ax.set_xlabel()
ax.set_ylabel()
ax.set_zlabel()

#设定刻度
ax.set_xticks()
ax.set_yticks()
ax.set_zticks()

#设定图题
ax.set_title()
```

[详细信息](https://matplotlib.org/stable/api/_as_gen/mpl_toolkits.mplot3d.axes3d.Axes3D.html)

## plt中的文本
使用text对象进行文本控制，可以通过位置来控制
```python
plt.text(x, y, z, text)
```

对于文本公式使用LateX时，使用```r'$LateX$'```来存放公式源码：

```python
plt.text(x, y, z, r'$e^i\pi-1=0$')
```

## 保存图片

plt提供了保存图片的工具:

```python
plt.savefig(fname, dpi=None, **kwags)
```

`fname`为字符串形式的路径（包含后缀），`dpi`为分辨率，[详情](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.savefig.html) 

当用作汇报、打印时，**推荐使用`.bmp`以及`.svg`格式**。当使用`.png`格式时，要考虑绘图导出后的图片无背景问题。


