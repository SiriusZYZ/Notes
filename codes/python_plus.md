# 变量
---
## 对象的拷贝

- `list().copy()` 返回的是一个浅拷贝对象，就是这边修改了那边也会一起修改
- 使用`copy` 模块的`copy.deepcopy(var)` 可返回深拷贝对象，修改该对象不会修改原来那个对象

# 装饰器
---
## Fact

我们注意[函数的特点](#函数的特点)，函数是一个对象，既可以作为另一个函数的参数，也可以作为函数返回的对象。装饰器的作用是实现对多个“函数的函数”进行简洁的语法包装

## 无参装饰器
- `@dec_func`修饰符加在`func1` 前，等价于`func1 = dec_func(func1)`
- 而`dec_func`相当于输入是函数，输出也是函数的一个函数
```python
def dec_func(func):
	def wrapper(*args, ** kwargs):
		...
		ret = func(*args, ** kwargs)
		...
		return ret

@dec_func
def my_func(*args, ** kwargs):
	...
```
- 首先，装饰器`@`后接的必须是装饰函数名`dec_func`，`dec_func`接受一个函数对象作为参数
- 一般在`dec_func` 中有一`wrapper`函数，该函数主要作为`dec_func`的返回对象
- `wrapper(*args, ** kwargs)` 的作用在于实现被装饰函数对象`my_func`变长参数

参考此例子:
```python
import time
def timeit(func):
    def wrapper(x):
        t0 = time.time()
        ret = func(x)
        print(time.time() - t0)
        return ret
    return wrapper

@timeit
def my_func(x):
    time.sleep(x)
    return "haha"

my_func(1)

>>> 1.0033631324768066

'haha'
```
- `timeit`接受函数`func`作为输入，其作用是在运行函数`func`时打印其运行时间
- `@timeit`装饰了`my_func`，也就等价于`my_func = timeit(my_func)`，进行了“函数的函数”包装
- 最终`my_func` 等价了:
```python
def my_func_internal(x):
    time.sleep(x)
    return "haha"
def my_func(x):
	t0 = time.time()
	ret = my_func_internal(x)
	print(time.time() - t0)
	return ret
```


## 带参装饰器

- 当装饰器接受参数时，装饰器语句返回了对应参数计算得到的新装饰器
- 带参情况下，`@dec_func(x)` 返回了一个新的装饰器
```python
def dec_func(*args, ** kwargs):
	def inner(func):
		def wrapper(*args, ** kwargs):
			...
			ret = func(*args, ** kwargs)
			...
			return ret
		return wrapper
	return func

@dec_func()
def my_func(*args, ** kwargs):
	...
```

> 例子：
```python
# timit(Iteration)作用是运行Iteration 次func

def timeit(Iteration):
    def inner(func):
        def wrapper(*args, **kwargs):
            t0 = time.time()
            for _ in range(Iteration):
                ret = func(*args, **kwargs)
            print(time.time() - t0)
            #return ret
        return wrapper
    return inner

# @timeit(Iteration) 是一个函数，接受一个整型变量，返回函数对象inner
# inner(func) 作为一个装饰器接受函数作为输入，并返回函数对象作为输出
# inner装饰了被装饰函数my_func1 和my_func2
@timeit(100000)
def my_func1(x):
    return x**2

@timeit(100000)
def my_func2(x, y):
    return x**y

>>> my_func1(2)
0.024982690811157227
>>> my_func2(2, 2.1)
0.024982690811157227
```



# 运算
---
## 位运算

位运算是把数字转换为机器语言，也就是二进制来进行计算的一种运算形式。**位运算符只能操作整数。**
- `a&b` 表示与运算：
- `a^b` 表示异或运算
- `a|b` 表示或运算
- `~ a` 表示按位取反
- `a << n` 表示按位左移`n` 位，高位丢弃低位补0
- `a >> n` 表示按位右移`n` 位，低位丢弃高位补0

以上操作均操作数的补码：
- 正数的补码与原码相同
- 负数的补码为除符号位全部按位取反后+1

**需要注意的是，位运算和python逻辑表达式中的`and` ,`or` 等不同。**



# 内置对象
---
## 字典
`dict().get(key_name, default)` 可以字典某个键值，当该键值不存在时返回`default`
```python
>>> a = dict()
>>> a.get('abc', 0)
0
```

`sorted(iterable, cmp=None, key=None, reverse=False)` 可以返回排序后的可迭代对象，不同于`list.sort()` 方法，这么做可以返回排序后的对象。
其中：
terable：是可迭代类型;
cmp：用于比较的函数（大于时返回1，小于时返回-1，等于时返回0），比较什么由key决定,有默认值，迭代集合中的一项;
key：用列表元素的某个属性和函数进行作为关键字，有默认值，迭代集合中的一项;
reverse：排序规则. reverse = True 或者 reverse = False，有默认值。


# 函数
---
## 函数的特点

1. python中万物皆为对象，function也不例外
```python
>>> a = lambda x: x**2
>>> a
<function <lambda> at 0x0000022018B648B8>
```
2. 由此，函数作为一个对象也可以被传入其他函数中
```python
>>> def cal_number(func, x):
...     print(func(x))
...
>>> cal_number(lambda x: x**3, 2)
8
```
3. 函数也可以作为函数返回的值
```python
>>> def get_pow_func(n):
...     def pow_func(x):
...             return x**n
...     return pow_func
...
>>> p2 = get_pow_func(2)
>>> p2(5)
25
>>> p3 = get_pow_func(3)
>>> p3(5)
125
```



## 可变参数

1. 可变参数应该跟在已经给出的参数后面，如:`func(a, b, *args, **kwargs):`
2. `*args` 表示此处有任意个无名参数，这些参数会被理解为一个元组
3. `**kwargs` 表示此处有任意个具名参数，这些参数会被理解为一个字典
4. `*args` 必须在 `**kwargs` 之前，不然会报错
5. 在函数内调用时，先分配已经固定的参数。再分配接下来的不具名的参数到元组`arg`中，接下来看有没有具名参数，如果有就分配至字典`kwarg` 中
![[Pasted image 20221222154705.png]]
6. 务必注意位置顺序，不具名参数要在具名参数之前:
 ![[Pasted image 20221222155011.png]]

## lambda
形式:
`lambda [argument_list]: expression`
展开以后是:
```python
def __(argument_list):
	return expression
```


# 判断
---
## 通过字典来实现switch
使用字典简易实现`switch`的工作:
```python
def func():
	...
switcher = {'case':func, ..}

switcher[case]([arguments])
```
其中的func表示了在case情况下应该执行的操作，可以使用lambda来实现，注意此处是传函数索引。

例如，实现四则运算:
```python
switcher = {'+':lambda x,y: x+y , '-':lambda x,y: x-y, '*':lambda x,y: x*y, '/':lambda x,y: x/y}

operator = '*'
switcher[operator](5, 6)
```


# 循环
---
## 针对for循环的循环对象改进

使用`_` 或`__` 来替代常用的`i` 作为循环计数:
```python
>>> for __ in range(5):
...     print('a')
...
a
a
a
a
a
```

使用`enumerate([iterabel])` 来创建一个产生索引和元素的迭代器
```python
>>> for i, char in enumerate('abcde'):
...     print(i,char)
0 a
1 b
2 c
3 d
4 e
```
