[标准库re](https://docs.python.org/zh-cn/3/library/re.html#module-re)
> 本模块提供了与 Perl 语言类似的正则表达式匹配操作。


# 正则表达式语法

基础语法详见[regex](../../misc/regex)

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
- 查找`string`中**所有**与`pattern`匹配的子串，以列表形式返回

```python
re.finditer(pattern, string, flags=0) -> Iterator
```
- 查找`string`中**所有**与`pattern`匹配的子串，以迭代器的形式返回

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

## 字符串匹配模式的替换

