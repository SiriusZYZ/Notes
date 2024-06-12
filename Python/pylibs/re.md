[标准库re](https://docs.python.org/zh-cn/3/library/re.html#module-re)
> 本模块提供了与 Perl 语言类似的正则表达式匹配操作。


# 正则表达式语法

基础语法详见[regex](../../misc/regex)

## `re`所支持的语法扩展


# re的修饰符

- [`re.A`](https://docs.python.org/zh-cn/3/library/re.html#re.A "re.A") (仅限 ASCII 匹配)
- [`re.I`](https://docs.python.org/zh-cn/3/library/re.html#re.I "re.I") (忽略大小写)
- [`re.L`](https://docs.python.org/zh-cn/3/library/re.html#re.L "re.L") (依赖于语言区域)
- [`re.M`](https://docs.python.org/zh-cn/3/library/re.html#re.M "re.M") (多行)
- [`re.S`](https://docs.python.org/zh-cn/3/library/re.html#re.S "re.S") (点号匹配所有字符)
- [`re.U`](https://docs.python.org/zh-cn/3/library/re.html#re.U "re.U") (Unicode 匹配)
- [`re.X`](https://docs.python.org/zh-cn/3/library/re.html#re.X "re.X") (详细)

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
- 由成功的 `rmatch` 和 `search` 所返回的匹配对象，可以理解为匹配的结果
