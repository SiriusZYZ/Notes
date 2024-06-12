[标准库re](https://docs.python.org/zh-cn/3/library/re.html#module-re)
> 本模块提供了与 Perl 语言类似的正则表达式匹配操作。

# 建议
- 捕获能提供比较好的regex语法扩展。为了更好地使用它，建议用`re.match`, `re.search` 等返回`re.Match` 对象的函数，而不是用`re.find`系列的函数。这是因为后者只会返回分组构成的元组，但前者可以返回匹配到的主分组、分组，还可以用标记的名字来返回分组

# 正则表达式语法

正则表达式基础语法详见[regex](/misc/regex)
## `re`的修饰符
- [`re.A`](https://docs.python.org/zh-cn/3/library/re.html#re.A "re.A") (仅限 ASCII 匹配)
- [`re.I`](https://docs.python.org/zh-cn/3/library/re.html#re.I "re.I") (忽略大小写)
- [`re.L`](https://docs.python.org/zh-cn/3/library/re.html#re.L "re.L") (依赖于语言区域)
- [`re.M`](https://docs.python.org/zh-cn/3/library/re.html#re.M "re.M") (多行)
- [`re.S`](https://docs.python.org/zh-cn/3/library/re.html#re.S "re.S") (点号匹配所有字符)
- [`re.U`](https://docs.python.org/zh-cn/3/library/re.html#re.U "re.U") (Unicode 匹配)
- [`re.X`](https://docs.python.org/zh-cn/3/library/re.html#re.X "re.X") (详细)

## `re`所支持的语法扩展

`re`支持的扩展语法，以`(?...)` 为主要形式
在扩展语法中，使用中括号`[]`来强制防转义，如`<>|=`
### 修饰符语法

`(?aiLmsux-imsx:…)`
	（零个或多个来自 `'a'`, `'i'`, `'L'`, `'m'`, `'s'`, `'u'`, `'x'` 集合的字母，后面可以带 `'-'` 再跟一个或多个来自 `'i'`, `'m'`, `'s'`, `'x'` 集合的字母。） 这些字母将为这部分表达式设置或移除相应的旗标
### 捕获

`(...)`
	捕获组合，`re`的组合除了能实现子模式以外，还能被在匹配结果中被单独捕获
```python
# 在主模式匹配成功后, 加了括号的组合会被分别捕获
>>> re.findall(r"(\w+) (\w+)", "i am very happy")
[('i', 'am'), ('very', 'happy')]
>>> re.findall(r'(\w+)=(\d+)', 'set width=20 and height=10')
[('width', '20'), ('height', '10')]

# 匹配过程中, 如果存在多层模式嵌套, 返回的结果则按照表达式从左到右, 结果以多叉树的前序遍历返回
>>> re.findall(r"((\w?)(\w+)) (\w+)", "You are very happy")
[('You', 'Y', 'ou', 'are'), ('very', 'v', 'ery', 'happy')]
# 第一个是((\w?)(\w+)), 第二个是(\w?), 第三个是(\w+), 第四个是(\w+)
```

`\id` 
	捕获的组合会被分配到一个`id`上，n个组从左到右依次被分配给`1~n`
	`\0` 是整个主表达式
	`\1` 是匹配的第一个分组


`(?:...)`
	非捕获组合，但是在匹配结果中不显示对应的子模式，相当于标准的子模式
```python
# 均不捕获子模式
>>> re.findall(r"(?:\w+) (?:\w+)", "You are very happy")
['You are', 'very happy']

# 捕获主模式的最后一个子模式
>>> re.findall(r"(?:\w+) (\w+)", "You are very happy")
['are', 'happy']
```

### 标记捕获

`(?P<name>...)`
	捕获`...` 子模式并将其标记为`name`
```python
>>> re.match(r"(?P<varname>\w+)=(?P<value>\d+)", "A=5 B=7").group("varname")
'A'
>>> re.match(r"(?P<varname>\w+)=(?P<value>\d+)", "A=5 B=7").group("value")
'5'
```

`(?P=name)`
	引用前面标记的`name`子模式匹配到的字符串作为当前的子模式
```python
# 引用前文的quote_marks来匹配单引号或双引号内的句子
>>> re.search(r"""(?P<quote_marks>['"])(.*?)(?P=quote_marks)""", "a'b'c").group()
"'b'"
```

| 引用组合 "quote" 的上下文            | 引用方法                                               |
| ---------------------------- | -------------------------------------------------- |
| 在正则式自身内                      | - `(?P=quote)` (如示)<br>- `\1`                      |
| 处理匹配对象 _m_                   | - `m.group('quote')`    <br>- `m.end('quote')` (等) |
| 传递到 `re.sub()` 里的 _repl_ 参数中 | - `\g<quote>`    <br>- `\g<1>`<br>- `\1`           |
|                              |                                                    |

### 注释

`(?#...)`
	注释语法，包含的内容会被忽略
```python
>>> re.findall(r"\w+(?#match some words)", "You are very happy")
['You', 'are', 'very', 'happy']
```


# 常用函数

## 模式构建函数
```python
re.compile(pattern: string, flags=0) -> re.Pattern
```
- 通过正则表达式构建一个模式对象`re.Pattern`

## 查找与模式匹配的字符串子串

```python
re.search(pattern, string, flags=0) -> re.Match, None
```
- 查找`string`中**首个**与`pattern`匹配的子串，成功返回`re.Match`，失败返回`None`

```python
re.findall(pattern, string, flags=0) -> List
```
- 查找`string`中**所有**与`pattern`匹配的子串，以**列表**形式返回

```python
re.finditer(pattern, string, flags=0) -> Iterator
```
- 查找`string`中**所有**与`pattern`匹配的子串，以**迭代器**的形式返回

## 确定字符串与模式是否匹配

```python
re.match(pattern, string, flags=0) -> re.Match, None
```
- 查找`string`**开头的若干字符**是否与`pattern`匹配，匹配则返回`re.Match`， 否则返回`None`

```python
re.fullmatch(pattern, string, flags=0) -> re.Match, None
```
- 确定`string`的**整个字符串**是否与`pattern`匹配，匹配则返回`re.Match`， 否则返回`None`

## 确定搜索结果

可以使用`if+re.` 的方式
```python
>>> if re.match("\w+ \w+", "hello world!"):
...     print("found a two-word-scentence!")
...
found a two-word-scentence!
```

## 字符串的分割

```python
re.split(pattern, string, maxsplit=0, flags=0) -> List
```
- 用`pattern`分开`string`
- `maxsplit`规定了最大分割次数(即切割次数)，当超过分割次数时，剩下的全部字符(包括匹配到的模式)全部返回到列表最后一个元素中
- `maxsplit` 是一个非负数，等于0时认为可以切割任意次数
## 字符串匹配模式的替换

```python
re.sub(pattern, repl:string, count=0, flags=0) -> string
re.sub(pattern, repl:function, count=0, flags=0) -> string
```
- 使用`repl`替代`string`中匹配到的最左边非重叠出现`pattern`
- `repl` 为字符串的时候就是直接代替
- `repl` 为函数时，接受`re.Match`作为参数，要求返回字符串，此时`re.sub`能根据`repl`返回的结果来替换匹配的`pattern`
- `count` 规定了最大替换次数，设置为0时含义为替换任意次数


```python
re.subn(pattern, repl:string, count=0, flags=0) -> tuple
re.subn(pattern, repl:function, count=0, flags=0) -> tuple
```
- 与`re.sub`类似，但是会返回一个元组`(result_string, replace_times)` 即包含`re.sub`的结果及模式的替换次数

```python
>>> re.sub(r"\B(?=(\d{3})+$)", ",", "1234567890")
'1,234,567,890'
>>> re.subn(r"\B(?=(\d{3})+$)", ",", "1234567890")
('1,234,567,890', 3)
```

## 字符串转义

```python
re.escape(pattern)
```
- 转义`pattern`中的特殊字符

```python
>>> re.escape('https://www.python.org')
'https://www\\.python\\.org'
```

# 对象

## `re.Pattern`
- 是由 `re.compile` 返回的已编译正则表达式对象，可以理解为模式

以下是成员变量
- `Pattern.pattern` 编译的原始样式字符串
- `Pattern.flags` 编译时的修饰符
- `Pattern.groups` 捕获到的模式串中组的数量

以下是成员函数
```python
re.Pattern.search(string, pos:Optional[int], endpos:Optional[int])-> re.Match
re.Pattern.match(string, pos:Optional[int], endpos:Optional[int])-> re.Match
re.Pattern.fullmatch(
					 string, 
					 pos:Optional[int], endpos:Optional[int]) -> re.Match
re.Pattern.split(string, maxsplit=0) -> List
re.Pattern.findall(
				   string, 
				   pos:Optional[int], endpos:Optional[int]) -> re.Match
re.Pattern.finditer(
				   string, 
				   pos:Optional[int], endpos:Optional[int]) -> re.Match
re.Pattern.sub(repl, string, count=0)
re.Pattern.subn(repl, string, count=0)
```
- 和`re` 其他函数比较像
- `pos` 为开始搜索的位置索引，默认`0`
- `endpos` 为结束搜索的位置索引

> 可选参数 _endpos_ 限定了字符串搜索的结束；它假定字符串长度到 _endpos_ ， 所以只有从 `pos` 到 `endpos - 1` 的字符会被匹配。如果 _endpos_ 小于 _pos_，就不会有匹配产生；另外，如果 _rx_ 是一个编译后的正则对象， `rx.search(string, 0, 50)` 等价于 `rx.search(string[:50], 0)`。


## `re.Match`
- 由成功的 `match` 和 `search` 所返回的匹配对象，可以理解为匹配的结果

```python
Match.group([group1, ...]) -> tuple, string
Match[group1] -> string
```
- 返回匹配到的表达式中所捕获的分组
- 重载了`__getitem__()` 语法，使得`Match`能够支持中括号索引访问返回分组
- `group1` 可以是分组序号，也可以是标记捕获时定义的变量名

```python
# group()直接返回匹配到的整个表达式, 不分组
>>> re.match(r"(\w+) (\w+)", "Isaac Newton, physicist").group()
'Isaac Newton'
# group(0)同group()
>>> re.match(r"(\w+) (\w+)", "Isaac Newton, physicist").group(0)
'Isaac Newton'
# >0时返回分组
>>> re.match(r"(\w+) (\w+)", "Isaac Newton, physicist").group(1)
'Isaac'
>>> re.match(r"(\w+) (\w+)", "Isaac Newton, physicist").group(2)
'Newton'
# 多个参数时, 逐一返回分组
>>> re.match(r"(\w+) (\w+)", "Isaac Newton, physicist").group(0,1,2)
('Isaac Newton', 'Isaac', 'Newton')
# 可以使用标记捕获来获取组
>>> m = re.match(r"(?P<first_name>\w+) (?P<last_name>\w+)", "Malcolm Reynolds")
>>> m.group('first_name')
'Malcolm'
>>> m.group('last_name')
'Reynolds'
```

```python
Match.groups() -> tuple
```
- 返回匹配到的所有分组

```python
Match.groupdict() -> dict
```
- 以子模式名为键，匹配到的子串为值返回一个字典
```python
>>> m = re.match(r"(?P<first_name>\w+) (?P<last_name>\w+)", "Malcolm Reynolds")
>>> m.groupdict()
{'first_name': 'Malcolm', 'last_name': 'Reynolds'}
```

