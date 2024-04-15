[标准库 time](https://docs.python.org/zh-cn/3/library/time.html)
# time

## 处理器时间

==**获取当前处理器时间**==
```python
time.process_time()
```
- `process_time()`此函数返回自第一次调用此函数以来经过的 wallclock 秒数，作为浮点数

## 纪元秒数
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

## 历法时间

==**获取当前时间结构体**==
```python
# GMT time_struct
time.gmtime()

# local time time_struct
time.localtime()
```
- `gmtime()` 获取当前GMT时间的结构体`time.struct_time`
- `localtime()` 获取当前本地时间的结构体`time.struct_time`

## 时间的输入和导出

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