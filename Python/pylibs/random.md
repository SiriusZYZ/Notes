[Python标准库--random](https://docs.python.org/zh-cn/3/library/random.html)

处理随机数相关,导入方法:
```python
import random
```

# 比特字符

生成长度为`n`个随机字节
```python
random.randbytes(n)
```
该方法不适用于生成安全凭据。

# 数字

生成0-1之间**随机浮点数**
```python
random.random()
```

生成`[a,b]`之间的**随机浮点数**
```python
random.uniform(a,b)
```

生成`[a,b]`之间的**随机整数**
```python
random.randint(a,b)
```

生成`range()`中随机一个数
```python
random.randrange(stop)
random.rangrange(start, stop, [, step])
```
# 序列

从可迭代对象`seq`中随机选取一数，包括`dict`, `tuple`及其他可以用`[]` 来获取的对象
```python
random.choice(seq)
```

从元祖、列表中无重复随机抽样`n`个数
```python
random.sample(seq, n)
```

就地打乱`seq`的顺序
```python
random.shuffle(seq)
```

返回打乱的新`seq`
```python
random.sample(seq, k = len(seq))
```

# 分布

| 函数                                                                             | 分布类型                                     |
| ------------------------------------------------------------------------------ | ---------------------------------------- |
| `random.random()`                                                              | `[0,1)` 中的**均匀分布**                       |
| `random.uniform(a, b)`                                                         | `[a, b]`中的**均匀分布**                       |
| `random.triangular(l, h, mode)`                                                | `[l,h]`中`mode` 的**对称分布**                 |
| `random.betavariate(alpha, beta)`                                              | `alpha`和`beta`下的**beta分布**               |
| `random.expovariate(lambd=1.0)`                                                | `lambd` 下的**指数分布**                       |
| `random.gauss(mu=0.0, sigma=1.0)`<br>`random.normalvariate(mu=0.0, sigma=1.0)` | 均值`mu`及标准差`sigam`下的**高斯分布**              |
| `random.gammavariate(alpha, beta)`                                             | shape=`alpha` 和 scale=`beta`下的$\gamma$分布 |
|                                                                                |                                          |
