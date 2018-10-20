# 1. 函数式编程

**函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数！**

## 1.高阶函数

**函数本身也可以赋值给变量，即：变量可以指向函数。**

### 1.传入函数

```
def add(x, y, f):
    return f(x) + f(y)
```

### 2. `map`函数 

**`map`将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回。**

```
list(map(str, [1,2,3,4,5]))
```

### 3. `reduce`函数

`reduce`把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算。

```
from functools import reduce
def add(x, y):
    return x * 10 + y
reduce(add, [1,3,5,7,9])
```

利用`reduce`函数和`map`函数将`char`转为`int`

```
from functools import reduce
digits = {'0' : 0, '1' : 1, '2' : 2, '3' : 3, '4' : 4, '5' : 5}
def char2int(s):
   def fn(x, y):
       return x * 10 + y
   def char2num(s):
        return digits[s]
    return reduce(fn, map(char2num, s))
```

***Practice***

利用`map()`函数，把用户输入的不规范的英文名字，变为首字母大写，其他小写的规范名字。输入：`['adam', 'LISA', 'barT']`，输出：`['Adam', 'Lisa', 'Bart']`：

```
# -*- coding: utf-8 -*-
def normalize(name):
    return str.upper(name[0]) + str.lower(name[1:])
L1 = ['adam', 'LISA', 'barT']
L2 = list(map(normalize, L1))
print(L2)
```

Python提供的`sum()`函数可以接受一个list并求和，请编写一个`prod()`函数，可以接受一个list并利用`reduce()`求积：

```
# -*- coding: utf-8 -*-
from functools import reduce
def prod(L):
    def fn(x,y):
        return x*y
return reduce(fn,L) 
```

***利用`map`和`reduce`编写一个`str2float`函数，把字符串`'123.456'`转换成浮点数`123.456`：***

```
# -*- coding: utf-8 -*-
from functools import reduce
digits = {'0' : 0, '1' : 1, '2' : 2, '3' : 3, '4' : 4, '5' : 5, '6' : 6, '7' : 7, '8' = 8, '9' = 9, '.' = -1}
def str2float(s):
    nums = map(lambda ch: digits[ch], s))
    point = 0
    def to_float(x,y)
        if y == -1:
           point = 1
           return x
        elif point = 0:
           return x*10+y
        else:
           point = point*10
           return x + y / point
    return reduce(to_float, nums, 0.0)
```   

### 4. `filter`函数

Python内建的`filter()`函数用于过滤序列,“筛选”函数

`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。

例：把一个序列中的空字符串删掉，可以这么写：

```
def not_empty(s):
    return s and s.strip() # s.strip()是将字符首尾的空格删掉，and是指s和s.strip()作与计算。
list(filter(not_empty, ['A', '', 'B', None, 'C', '  ']))
# 结果: ['A', 'B', 'C']
```

用`filter`求素数：

```
# -*- coding: utf-8 -*-
def _odd_iter():
    n = 1 
    while True:
          n = n + 2
          yield n
def _not_divisible(n):
    return lambda x: x % n > 0
def primes():
    yield 2
    it = _odd_iter()
    while True:
        n = next(it)
        yield n
        it = filter(_not_dicisible(n), it)
for n in primes():
    if n < 1000:
       print(n)
    else:
       break
```