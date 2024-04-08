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

如果保存的图片没背景，需要在`fig` 对应的图中加入:
```python
plt.figure(
		   figsize = (,),
		   face_color = "w",  # 白色背景
		   edge_color = "k"   # 黑色边线
)
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
