[标准库`collections`](https://docs.python.org/zh-cn/3/library/collections.html#module-collections "collections: Container datatypes") : 包含更多数据结构类型


这个模块实现了一些专门化的容器，提供了对 Python 的通用内建容器 [`dict`](https://docs.python.org/zh-cn/3/library/stdtypes.html#dict "dict")、[`list`](https://docs.python.org/zh-cn/3/library/stdtypes.html#list "list")、[`set`](https://docs.python.org/zh-cn/3/library/stdtypes.html#set "set") 和 [`tuple`](https://docs.python.org/zh-cn/3/library/stdtypes.html#tuple "tuple") 的补充。

|   |   |
|---|---|
|[`namedtuple()`](https://docs.python.org/zh-cn/3/library/collections.html#collections.namedtuple "collections.namedtuple")|一个工厂函数，用来创建元组的子类，子类的字段是有名称的。|
|[`deque`](https://docs.python.org/zh-cn/3/library/collections.html#collections.deque "collections.deque")|类似列表的容器，但 append 和 pop 在其两端的速度都很快。|
|[`ChainMap`](https://docs.python.org/zh-cn/3/library/collections.html#collections.ChainMap "collections.ChainMap")|类似字典的类，用于创建包含多个映射的单个视图。|
|[`Counter`](https://docs.python.org/zh-cn/3/library/collections.html#collections.Counter "collections.Counter")|用于计数 [hashable](https://docs.python.org/zh-cn/3/glossary.html#term-hashable) 对象的字典子类|
|[`OrderedDict`](https://docs.python.org/zh-cn/3/library/collections.html#collections.OrderedDict "collections.OrderedDict")|字典的子类，能记住条目被添加进去的顺序。|
|[`defaultdict`](https://docs.python.org/zh-cn/3/library/collections.html#collections.defaultdict "collections.defaultdict")|字典的子类，通过调用用户指定的工厂函数，为键提供默认值。|
|[`UserDict`](https://docs.python.org/zh-cn/3/library/collections.html#collections.UserDict "collections.UserDict")|封装了字典对象，简化了字典子类化|
|[`UserList`](https://docs.python.org/zh-cn/3/library/collections.html#collections.UserList "collections.UserList")|封装了列表对象，简化了列表子类化|
|[`UserString`](https://docs.python.org/zh-cn/3/library/collections.html#collections.UserString "collections.UserString")|封装了字符串对象，简化了字符串子类化|

# deque 双向队列

Deque 队列是对栈或 queue 队列的泛化（该名称的发音为 "deck"，是 "double-ended queue" 的简写形式)。 Deque 支持线程安全，高度节省内存地从 deque 的任一端添加和弹出条目，在两个方向上的大致性能均为 _O_(1)。

> `deque` 是一种线性表，在计算时间复杂度时必须注意

构造函数
```python
dq = collections.deque()
dq = collections.deque([iterable, ])
dq = collections.deque([iterable, [, maxlen]]) # maxlen指定了最大长度
```

| 常用成员                  | 释义             | 备注              |
| --------------------- | -------------- | --------------- |
| `maxlen: int`         | 最大长度           | 未指定时为`None`     |
| `append(x)`           | 右端加入`x`        |                 |
| `extend(seq)`         | 右端加入可迭代序列`seq` |                 |
| `appendleft(x)`       | 左端加入`x`        |                 |
| `extendleft(seq)`     | 左端加入可迭代序列`seq` |                 |
| `pop()-> obj`         | 弹出右端一个元素       | 如空则`IndexError` |
| `popleft() -> obj`    | 弹出左端一个元素       | 如空则`IndexError` |
| `reverse() -> None`   | 队列取反           |                 |
| `rotate(n=1) -> None` | 向右循环移动`n`步     | `n<0` 时为向左      |
| `clear() -> None`     | 移除所有元素         |                 |
| `copy() -> "deque"`   | 创建浅拷贝，(3.5+)   |                 |
| `count(x) -> int`     | 查询`x` 个数       |                 |
| `__get__(idx)`        | 支持索引访问         | `dq[idx]`       |

# defaultdict
