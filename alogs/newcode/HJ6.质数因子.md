#华为 
功能:输入一个正整数，按照从小到大的顺序输出它的所有质因子（重复的也要列举）（如180的质因子为2 2 3 3 5 ）

  

数据范围： $1≤n≤2×10^9+14 1≤n≤2×10^9+14$

### 输入描述：

输入一个整数

### 输出描述：

按照从小到大的顺序输出它的所有质数的因子，以空格隔开。

## 示例1

输入：
```
180
```
输出：
```
2 2 3 3 5
```

## 思路
- 本题关键在于意识到，在循环除2直到不能整除2以后，就不用考虑包含2的合数了。
```python
import math

def handle(x) -> None:

    if x == 1: 
        print(1)
        return

    min_prime = 2

    for i in range(2, int(math.sqrt(x)) + 1):
        while(x % i == 0):
            x /= i
            print(i, end = ' ')
    
    if int(x) == 1: 
        return
    else:
        print(int(x))
            

handle(int(input()))
```