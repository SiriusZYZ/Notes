- [`cProfile`](https://docs.python.org/zh-cn/3/library/profile.html#module-cProfile "cProfile") 和 [`profile`](https://docs.python.org/zh-cn/3/library/profile.html#module-profile "profile: Python source profiler.") 提供了 Python 程序的 _确定性性能分析_ 。 _profile_ 是一组统计数据，描述程序的各个部分执行的频率和时间。这些统计数据可以通过 [`pstats`](https://docs.python.org/zh-cn/3/library/profile.html#module-pstats "pstats: Statistics object for use with the profiler.") 模块格式化为报表。
# CProfile

Python 标准库提供了同一分析接口的两种不同实现：

1. 对于大多数用户，建议使用 [`cProfile`](https://docs.python.org/zh-cn/3/library/profile.html#module-cProfile "cProfile") ；这是一个 C 扩展插件，因为其合理的运行开销，所以适合于分析长时间运行的程序。该插件基于 `lsprof` ，由 Brett Rosen 和 Ted Chaotter 贡献。

2. [`profile`](https://docs.python.org/zh-cn/3/library/profile.html#module-profile "profile: Python source profiler.") 是一个纯 Python 模块（[`cProfile`](https://docs.python.org/zh-cn/3/library/profile.html#module-cProfile "cProfile") 就是模拟其接口的 C 语言实现），但它会显著增加配置程序的开销。如果你正在尝试以某种方式扩展分析器，则使用此模块可能会更容易完成任务。该模块最初由 Jim Roskind 设计和编写。


profiler 分析器模块被设计为给指定的程序提供执行概要文件，而不是用于基准测试目的（ [`timeit`](https://docs.python.org/zh-cn/3/library/timeit.html#module-timeit "timeit: Measure the execution time of small code snippets.") 才是用于此目标的，它能获得合理准确的结果）。这特别适用于将 Python 代码与 C 代码进行基准测试：分析器为Python 代码引入开销，但不会为 C级别的函数引入开销，因此 C 代码似乎比任何Python 代码都更快。


要分析采用单个参数的函数，可以执行以下操作：
```python
import cProfile
CProfile.run("Command", sort=.) # 执行Command的确定性性能分析, 并形成报表按某种顺序排序
```

函数帮助为:
```
Help on function run in module cProfile:

run(statement, filename=None, sort=-1)
    Run statement under profiler optionally saving results in filename

    This function takes a single argument that can be passed to the
    "exec" statement, and an optional file name.  In all cases this
    routine attempts to "exec" its first argument and gather profiling
    statistics from the execution. If no file name is present, then this
    function automatically prints a simple profiling report, sorted by the
    standard name string (file/line/function-name) that is presented in
    each line.
```

### 案例和报表含义

```python

cProfile.run('re.compile("foo|bar")',sort=1)
```
```
      214 function calls (207 primitive calls) in 0.002 seconds

Ordered by: cumulative time

ncalls  tottime  percall  cumtime  percall filename:lineno(function)
     1    0.000    0.000    0.002    0.002 {built-in method builtins.exec}
     1    0.000    0.000    0.001    0.001 <string>:1(<module>)
     1    0.000    0.000    0.001    0.001 __init__.py:250(compile)
     1    0.000    0.000    0.001    0.001 __init__.py:289(_compile)
     1    0.000    0.000    0.000    0.000 _compiler.py:759(compile)
     1    0.000    0.000    0.000    0.000 _parser.py:937(parse)
     1    0.000    0.000    0.000    0.000 _compiler.py:598(_code)
     1    0.000    0.000    0.000    0.000 _parser.py:435(_parse_sub)
```

其中返回结果按顺序显式如下：

| 名称                            | 含义          | 备注                                                                         |
| ----------------------------- | ----------- | -------------------------------------------------------------------------- |
| ncalls                        | 调用次数        | 如果有两个数字（例如3/1），则表示函数递归。<br>第二个值是原始调用次数，第一个是调用的总次数。<br>请注意，当函数不递归时，只打印单个数字。 |
| tottime                       | 指定函数中消耗的总时间 | 不包括调用子函数的时间。与cumtime区别                                                     |
| percall                       | 单次调用消耗时间    | `tottime` 除以 `ncalls` 的商                                                   |
| cumtime                       | 指定函数中消耗的总时间 | 函数从调用到退出的全部时间。<br>这个数字对于递归函数来说是准确的。                                        |
| percall                       | 单次调用消耗时间    | `cumtime` 除以原始调用（次数）的商                                                     |
| filename:<br>lineno(function) | 提供相应数据的每个函数 |                                                                            |
