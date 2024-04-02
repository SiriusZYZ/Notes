# 模块

## 基本语法

### import

```python
import pckg
```
- `import` 将接下来的包`pckg`作为模块导入，使用时需加前缀包名前缀，即`pckg.attributes`

```python
import pckg as alias
```
- `import` 将接下来的包`pckg`以别名`alias`作为模块导入，并以使用时需加别名前缀，即`alias.attributes`

实际上，`import` 是一种赋值语句，将一整个模块赋给某个变量。
```python
>>> import numpy
>>> type(numpy)
<class 'module'>
```

必须注意的是，`import` 后的`pckg.attr` 被改变时是全局的。以下例子中，c中有一个变量为`c.x=3`，`b.py`使用`import` 导入`c`，并修改`c.x=5`。`a.py` 使用`import` 导入`b`和`c`。由于导入过程中会执行文件，因此`b`中的修改会导致`a` 读取`c.x`时变量被更改。
不过这种`b`中的更改只会在内存中作用，不会最终作用到`c` 的具体源码或字节码中。
```python
# a.py
import b # or from b import *
import c
print(c.x)

# b.py
import c
c.x = 5

# c.py
x = 3

#% ipy
python a.py
>>> 5
```
### from
```python
from pckg import attr1, attr2
```
- `from ... import` 将`pckg.attr1` 及`pckg.attr2` 导入，不需要加包名前缀，调用直接使用`attr1` 及`attr2` 即可

```python
from pckg import attr1 as a1
```
- `from ... import` 将`pckg.attr`  导入并赋给别名`a1`

```python
from pckg import *
```
- `from ... import *` 将`pckg` 包中所有内容导入，且不需要加包名前缀

### exec

有时，存在一些你想导入字符串变量指代的模块。
比如:
```python
>>> target_module = "math"
>>> import target_module
```
但实际不行，`import`语句会把`target_module`解析为某个在搜索路径中的模块名，而非本文件中定义的字符串对象`target_module : string` 

要解决这个问题，使用`exec()`。`exec()`
```python
>>> target_module = "math"
>>> exec(f"import {target_module}")
```
`exec()` 有种在python中执行python的感觉，它接受一个字符串，该字符串被视为一行Python代码并在当前作用域下执行。
## 模块的跨目录调用

在跨目录调用时，首先铭记模块导入的路径优先级顺序，见[模块搜索路径](#模块搜索路径)

- 在程序执行的根目录下：
```shell
sl$ tree -P *.py
.
├── a.py
└── dir1
    ├── dir1_1
    │   ├── b.py
    │   ├── c.py
    │   ├── __init__.py
    ├── __init__.py
```

> **==同一根目录下的单层调用==**
> 如果`b` 想要调用`c` ，有两种方式，可以直接使用文件名作为包名
```python
# @ b.py
# 1 import
import c

# 2 from import
from c import *
```

> ==**向子目录文件的单层调用**==
> 如果`a` 想要调用`c`，应以`.` 的方式表达文件夹层次关系。
> >对于Python 版本低于3.3，要在`dir1`和`dir1_1`中加入`__init__.py` 令Python将`./dir1`和`./dir1/dir1_1`视为模块。这种做法是必须的，如果导包路径上任意文件夹下没有`__init__.py`，会导致错误。
```python
# @a.py
#1 import 
import utils.module1.b

#2 from
from utils.module1.b import *
```

>==包管理中的`__init__.py`==
>包管理中，文件之间的调用方式不同了
>1. `__init__.py` 的主要作用是将同一目录下的各源文件关联起来，并使用同一的逻辑命名空间(目录名)。
>   > 这种做法的好处是把一组源码视为一个整体，可以避免不同模块之间存在同名文件导致不知道用哪个的情况
>2. 如果该文件夹被看作一个包，则应该使用`.` 这种相对路径方式和`from`来进行包整合。在`__init__.py` 可以使用`from` 来进行包导入，并使用`.` 来指代当前目录($\approx$`./`)。
>   > 相对路径导入是为了解决包内文件可能与搜索路径中的其他源码文件重名的情况
>   > 相对导入的作用域只适用于
>3. `__init__.py` 中的`from` 存在两种载入其他文件的方式:
>   >将同目录下文件也视为一个模块导入，例如`matplotlib.pyplot` 这种模式
```python
# @ dir1/dir1_1/__init__.py
from . import b # 将同一文件夹下b.py 的内容包含到__init__.py

# @ a.py
import dir1.dir1_1 as m
m.b.attrs  # 需要指出b才能调用b内部的东西
dir(m)
>>> ['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'b'] # 此处b 是一个模块
```
> > 将同目录下文件内容全部导入，不视为模块。这种方法可以把较深层次中的一些源码直接引入到包中，在主脚本`import`时就不用写很长一段导包路径
```python
# @ dir1/dir1_1/__init__.py
from .b import * # 将同一文件夹下b.py 的内容包含到__init__.py

# @ a.py
import dir1.dir1_1 as m
m.attrs  # 不需要指出b就能调用b中的东西, 这是因为__init__.py已经把这些东西当成模块的全局attrs了
dir(m)
>>> ['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'b的所有attrs...']
```

## 高级模块话题

> ==**模块中可以显式指定能被`from`导入和不能的对象**==
> 指定模块中能被导入的对象能最大化减少在主函数`from import *`的影响
> 1. `__all__: list` 列表变量指定了本模块中所有可以由`from .. import *` 时能被导入的变量名。如:
> 2. `_X: list` 列表变量指定了本模块中所有不能由`from .. import *` 时能被导入的变量名。
> 3. `_Varaiable_Name` 使用前加下划线的变量名可以直接避免被导入

> ==**`__name__`**==和==**`"__main__"`**==
> 每个模块(或文件)都有`__name__`自动生成的内置属性
> 1. 如果文件作为顶层程序文件启动，则会设置为`"__main__"` 
> 2. 如果文件作为模块被导入，则会被设置为作出导入行为的文件角度来看的模块名
> 通常可以用这个方法进行自我测试，来验证本文件是要被执行还是作为包导入:
```python
# @__init__.py
pass

if __name__ == "__main__":
	print("This is a module entry.")
```


## 模块导入的技术细节

> 与C不同，Python的导入并非把一个文件的文本插入另外一个文件，而是运行时的运算。

### sys.modules

当模块被导入时，首先检查`sys.modules`的字典。该字典**将模块名称与已经加载的模块一一映射**。要迭代`sys.modules`，建议使用`sys.modules.copy()`而非直接迭代。这能避免其他线程活动影响该字典的大小，导致`for` 语句的迭代器出错。

### 模块搜索路径

按优先级次序包括
1. 程序的主目录 
2. [`PYTHONPATH`](https://docs.python.org/zh-cn/3/using/cmdline.html#envvar-PYTHONPATH)
3. 标准库目录
4. `.pth` 目录
这种优先级也意味着，同名模块中优先级高的会被更早导入，因此要注意模块重名情况。
可以使用`sys` 模块的`path` 变量查看搜索路径
```python
>>> sys.path
['', 'C:\\Users\\admin\\AppData\\Local\\Programs\\Python\\Python311\\python311.zip', 'C:\\Users\\admin\\AppData\\Local\\Programs\\Python\\Python311\\DLLs', 'C:\\Users\\admin\\AppData\\Local\\Programs\\Python\\Python311\\Lib', 'C:\\Users\\admin\\AppData\\Local\\Programs\\Python\\Python311', 'C:\\Users\\admin\\AppData\\Roaming\\Python\\Python311\\site-packages', 'C:\\Users\\admin\\AppData\\Roaming\\Python\\Python311\\site-packages\\win32', 'C:\\Users\\admin\\AppData\\Roaming\\Python\\Python311\\site-packages\\win32\\lib', 'C:\\Users\\admin\\AppData\\Roaming\\Python\\Python311\\site-packages\\Pythonwin', 'C:\\Users\\admin\\AppData\\Local\\Programs\\Python\\Python311\\Lib\\site-packages']
```
### 首次导入
如果是首次导入时，会执行:
1. 找到对应的模块文件
2. 编译字节码（可选），这将产生`__pycache__`中的`.pyc` 文件。当`.pyc` 文件时间比目标源码`.py` 时间早，说明可能发生了更改，需要重新编译。否则不编译。
   > 如果模块中只发现了`.pyc` 而没有源码`.py` 依旧能完成导入。这种情况下，可以只发布`.pyc` 而不发布`.py` 来隐藏你的包技术细节。
3. 执行模块的所有代码来创建定义的对象。

### 再次导入和重新导入

当已经导入模块，后续再导入模块时就不会执行模块中的代码。因此在进程导入模块后修改模块源码，将不会反映到进程中。
要想重新导入，需要使用`importlib.reload(m: module)`，该函数接受一个模块对象，参考[`import`的本质是赋值](#import)
比如:
```python
import importlib.reload
import numpy

importlib.reload(numpy)
```


# 变量
---
## 对象的拷贝

- `list().copy()` 返回的是一个浅拷贝对象，就是这边修改了那边也会一起修改
- 使用`copy` 模块的`copy.deepcopy(var)` 可返回深拷贝对象，修改该对象不会修改原来那个对象

# 封包和解包
---
- **把多个值赋给一个变量时，Python会自动的把多个值封装成元组**，称为序列封包。
- **把一个序列（列表、元组、字符串等）直接赋给多个变量，此时会把序列中的各个元素依次赋值给每个变量**，但是元素的个数需要和变量个数相同，这称为序列解包。
## 封包的情况

- 把多个值赋给一个变量时  
```python
>>> a = 1,2,3,4
>>> a
(1, 2, 3, 4)
```

- 函数返回多个值时，其返回真实的东西其实是一个元组，就是把多个返回值封包了
```python
>>> def A(x, y):
...     return x+y, x-y
...
>>> type(A(1,2))
<class 'tuple'>
```

## 解包的情况

- 我们可以把数个变量对应等量的迭代对象
```python
>>> a, b, c, d = [1, 2, 3, 4]
>>> print(a, b, c, d)
1 2 3 4
>>> a, b, c, d = "opqr"
>>> print(a, b, c, d)
o p q r
```

- 当然也可以用来解包多值返回的函数输出
```python
>>> def func(x):
...     return x*x, x**x
...
>>> x_2, x_x = func(5)
>>> print(x_2, x_x)
25 3125
```

## `*`运算符和解封包
[函数传参的解封包](#可变参数):
> 1. 可变参数应该跟在已经给出的参数后面，如:`func(a, b, *args, **kwargs):`
> 2. `*args` 表示此处有任意个无名参数，这些参数会被理解为一个元组
> 3. `**kwargs` 表示此处有任意个具名参数，这些参数会被理解为一个字典
> 4. `*args` 必须在 `**kwargs` 之前，不然会报错
> 5. 在函数内调用时，先分配已经固定的参数。再分配接下来的不具名的参数到元组`arg`中，接下来看有没有具名参数，如果有就分配至字典`kwarg` 中


- 通过在解包过程中某个变量前加 **`*` 可以把该变量转换为列表**:
- 使用这种方式注意事项是：
  1. 左值序列中只有一个变量带星
  2. 由以下最后一个样例可以看出，带`*`参数是最后被赋值的，所以有可能出现该参数得到不止一个值，或得到0个值成为空列表 
```python
>>> a, b, c = [1,2,3]
>>> print(a, b, c)
1 2 3
>>> a, *b, c = [1,2,3]
>>> print(a, b, c)
1 [2] 3
>>> a, *b, c = [1,2,3,4]
>>> print(a, b, c)
1 [2, 3] 4

# 注意这一个样例
>>> a, *b, c = [1,2]
>>> print(a, b, c)
1 [] 2
```


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

