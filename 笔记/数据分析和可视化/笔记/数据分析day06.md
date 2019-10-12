# 数据分析DAY06

[TOC]

### 符号数组

sign函数可以把样本数组变成对应的符号数组。正数转换为1，负数转换换为-1，0不变。

```python
ary = np.sign(array)
```

**OBV能量潮 (净额成交量)**

1、投资者对股价的评论越不一致，成交量越大；反之，成交量就小。因此，可用成交量来判断市场的人气和多空双方的力量。

2、重力原理。上升的物体迟早会下跌，而物体上升所需的能量比下跌时多。涉及到股市则可解释为：一方面股价迟早会下跌；另一方面，股价上升时所需的能量大，因此股价的上升特别是上升初期必须有较大的成交量相配合；股价下跌时则不必耗费很大的能量，因此成交量不一定放大，甚至有萎缩趋势。

3、惯性原则——动则恒动、静则恒静。只有那些被投资者或主力相中的[热门股](https://baike.baidu.com/item/%E7%83%AD%E9%97%A8%E8%82%A1)会在很大一段时间内[成交量](https://baike.baidu.com/item/%E6%88%90%E4%BA%A4%E9%87%8F)和股价的波动都比较大，而无人问津的[冷门股](https://baike.baidu.com/item/%E5%86%B7%E9%97%A8%E8%82%A1)，则会在一段时间内，成交量和股价[波幅](https://baike.baidu.com/item/%E6%B3%A2%E5%B9%85)都比较小。

**应用规则**

1、当[股价](https://baike.baidu.com/item/%E8%82%A1%E4%BB%B7)上升而OBV线下降，表示[买盘](https://baike.baidu.com/item/%E4%B9%B0%E7%9B%98)无力，股价可能会回跌。

2、股价下降时而OBV线上升，表示买盘旺盛，逢低接手强股，股价可能会止跌回升。

3、OBV线缓慢上升，表示买气逐渐加强，为买进信号。

4、OBV线急速上升时，表示力量将用尽为[卖出信号](https://baike.baidu.com/item/%E5%8D%96%E5%87%BA%E4%BF%A1%E5%8F%B7)。

**绘制OBV能量潮**

若相比上一天的收盘价上涨，则为正成交量；若相比上一天的收盘价下跌，则为负成交量。

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo01_obv.py  净额成交量
若相比上一天的收盘价上涨，则为正成交量；
若相比上一天的收盘价下跌，则为负成交量。
"""
import numpy as np
import matplotlib.pyplot as mp
import datetime as dt
import matplotlib.dates as md


def dmy2ymd(s):
    dmy = str(s, encoding='utf-8')
    time = dt.datetime.strptime(dmy, '%d-%m-%Y')
    ymd = time.date().strftime('%Y-%m-%d')
    return ymd

# 读取文件数据
dates, closing_prices, volumns = np.loadtxt(
    '../da_data/aapl.csv', unpack=True,
    delimiter=',', usecols=(1, 6, 7),
    dtype='M8[D], f8, f8',
    converters={1: dmy2ymd})

# 绘制收盘价的折线图
mp.figure('OBV', facecolor='lightgray')
mp.title('OBV', fontsize=14)
mp.xlabel('Date', fontsize=12)
mp.ylabel('Volumns', fontsize=12)
mp.grid(linestyle=':')
mp.tick_params(labelsize=10)
# 设置刻度定位器
ax = mp.gca()
# 主刻度定位器为WeekdayLocator，每周一为主刻度
ax.xaxis.set_major_locator(
    md.WeekdayLocator(byweekday=md.MO))
ax.xaxis.set_major_formatter(
    md.DateFormatter('%d %b %Y'))
# 设置次刻度定位器为日定位器
ax.xaxis.set_minor_locator(md.DayLocator())

# 把dates转换为matplotlib识别的datetime类型
dates = dates.astype(md.datetime.datetime)

# 绘制OBV
diff_prices = np.diff(closing_prices)
sign_prices = np.sign(diff_prices)
obvs = volumns[1:] * sign_prices

# 把柱子都朝上， 并设置不同的颜色
# c = np.array(['red' if x == 1 else 'green'
#             for x in sign_prices])
c = np.zeros_like(sign_prices).astype('str_')
c[sign_prices >= 0] = 'red'
c[sign_prices == -1] = 'green'

mp.bar(dates[1:], np.abs(obvs), 0.9,
       color=c, label='OBV')

mp.legend()
mp.gcf().autofmt_xdate()
mp.tight_layout()
mp.show()
```

**数组处理函数**

```python
ary = np.piecewise(原数组，条件序列，取值序列)
# 对ary中的每个元素进行判断，若元素符合第一个条
# 件，则取值为0， 若符合第二个条件，则取值为1.
r = np.piecewise(ary, 
      [ary<60, ary>=60],
      [0, 1])
```

### 矢量化

简单理解，矢量化操作是指用数组代替标量来操作数组中的每个元素。

numpy提供了vectorize函数可以实现普通函数矢量化，普通函数只能处理标量，矢量化后的函数可以直接接收数组参数，那么最终结果也将会返回一个数组。

```python
def foo(a, b):
    return m.sqrt(a**2 + b**2)
# 普通调用
print(foo(3, 4))
# 传递数组
a = np.arange(3, 10)
b = np.arange(4, 11)
# print(foo(a, b))
# 函数矢量化
print(np.vectorize(foo)(a, b))

# 也可以使用frompyfunc函数实现函数矢量化操作
# frompyfunc(函数名, 接收参数的个数, 返回值个数)
print(np.frompyfunc(foo, 2, 1)(a, b))
```

案例：定义一种买入卖出策略，通过历史数据验证这种策略是否值得实施。

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo03_vec.py
定义一种买入卖出策略，
通过历史数据验证这种策略是否值得实施。
"""
import numpy as np
import matplotlib.pyplot as mp
import datetime as dt
import matplotlib.dates as md


def dmy2ymd(s):
    dmy = str(s, encoding='utf-8')
    time = dt.datetime.strptime(dmy, '%d-%m-%Y')
    ymd = time.date().strftime('%Y-%m-%d')
    return ymd

# 读取文件数据
dates, opening_prices, highest_prices,\
    lowest_prices, closing_prices = np.loadtxt(
        '../da_data/aapl.csv', unpack=True,
        delimiter=',', usecols=(1, 3, 4, 5, 6),
        dtype='M8[D], f8, f8, f8, f8',
        converters={1: dmy2ymd})


def profit(opening_price, highest_price,
           lowest_price, closing_price):
    # 定义一种投资策略，计算并返回该策略的收益率
    buying_price = opening_price * 0.99
    if lowest_price <= buying_price <= \
            highest_price:
        return (closing_price - buying_price) \
            * 100 / buying_price
    return np.nan  # not a number 无效数字


# 绘制收益率的折线图
mp.figure('Profits', facecolor='lightgray')
mp.title('Profits', fontsize=14)
mp.xlabel('Date', fontsize=12)
mp.ylabel('Profits', fontsize=12)
mp.grid(linestyle=':')
mp.tick_params(labelsize=10)
# 设置刻度定位器
ax = mp.gca()
# 主刻度定位器为WeekdayLocator，每周一为主刻度
ax.xaxis.set_major_locator(
    md.WeekdayLocator(byweekday=md.MO))
ax.xaxis.set_major_formatter(
    md.DateFormatter('%d %b %Y'))
# 设置次刻度定位器为日定位器
ax.xaxis.set_minor_locator(md.DayLocator())

# 矢量化profit函数，使得可以处理数组参数
profits = np.vectorize(profit)(
    opening_prices, highest_prices,
    lowest_prices, closing_prices)
# 拿到无效值的掩码
nan = np.isnan(profits)
dates, profits = dates[~nan], profits[~nan]
# 把dates转换为matplotlib识别的datetime类型
dates = dates.astype(md.datetime.datetime)
mp.plot(dates, profits, c='dodgerblue',
        linewidth=2, linestyle='-',
        label='Profits')
mp.hlines(profits.mean(), dates[0], dates[-1],
          color='orangered', label='AVG(Profits)')

mp.legend()
mp.gcf().autofmt_xdate()
mp.tight_layout()
mp.show()
```

### 矩阵

矩阵是matrix类型的对象，该类继承自ndarray。任何针对多维数组的操作，对矩阵对象同样有效。 并且作为子类matrix又结合了自身的特点，做了必要的扩充。比如：矩阵的乘法、矩阵求逆等。

**矩阵对象的创建**

```python
m = np.matrix(
	ary,		# 任何可以被解释为矩阵的容器
    copy=True   # 是否复制一份新的数据
)
```

```python
# 等价于 np.matrix(ary, copy=False)
np.mat(ary) 	# 任何可以被解释为矩阵的容器
```

```python
# 拼块字符串的规则：
# '1 2 3; 4 5 6; 7 8 9'
np.mat(拼块字符串)
```

**矩阵的乘法运算**

```python
a = np.mat('1 2; 3 4')
'''
			1  2 
	x		3  4
y
	1  2	7 10	
	3  4 15 22
'''
print(a * a)
```

**矩阵的逆矩阵**

若两个矩阵A、B满足：AB=BA=E （E为单位矩阵），则A、B互为逆矩阵。
$$
E(单位矩阵)=\left[\begin{array}{c}
1 & 0 & 0\\
0 & 1 & 0\\
0 & 0 & 1\\
\end{array}\right]
$$
使用numpy获取矩阵的逆：

```python
m = np.mat('1 2 3; 2 3 1; 2 2 3')
m.I
m * m.I -> E
```

案例：假设一帮孩子和家长去旅游，去程坐的大巴，小孩票价3元，大人票价3.2元，共花了118.4； 回程做火车，小孩票价3.5元，大人票价3.6元，共花了135.2。求大人和小孩的人数。
$$
\left[\begin{array}{c}
3 & 3.2\\
3.5 & 3.6\\
\end{array}\right]
\times
\left[\begin{array}{c}
x\\
y\\
\end{array}\right]
=
\left[\begin{array}{c}
118.4\\
135.2\\
\end{array}\right]
$$

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo04_mat.py 矩阵案例
"""
import numpy as np

a = np.arange(1, 10).reshape(3, 3)
print(a, type(a))
m = np.matrix(a, copy=False)
print(m, type(m))
a[0, 0] = 999
print(a, type(a))
print(m, type(m))

# 使用字符串拼块规则构建矩阵对象
m = np.mat('12 56 34; 65 56 75')
print(m, type(m))

# 测试矩阵的乘法
print('-' * 45)
a = np.arange(1, 7).reshape(2, 3)
b = a.copy()
print(a * b)
a = np.mat(a)
b = np.mat(b)
print(a * b.T)
print(a.dot(b.T))

# 测试矩阵的逆矩阵
print('-' * 45)
# 针对方阵的逆矩阵
m = np.mat('1 2 3; 2 3 1; 2 2 3')
print(m)
print(m.I)
print(m * m.I)
# 针对非方阵的逆矩阵  (广义逆矩阵)
m = np.mat('1 2 3; 2 3 1')
print(m)
print(m.I)
print(m * m.I)

# 应用题：
print('-' * 45)
prices = np.mat('3 3.2; 3.5 3.6')
totals = np.mat('118.4; 135.2')
x = np.linalg.lstsq(prices, totals)[0]
print(x)
x = np.linalg.solve(prices, totals)
print('solve:', x)


persons = prices.I * totals
print(persons)

# 斐波那契数列  1 1 2 3 5 8 13 ......
print('-' * 45)
'''
     1 1   1 1   1 1
     1 0   1 0   1 0
--------------------------------------------
1 1  2 1   3 2   5 3
1 0  1 1   2 1   3 2    

x^1  x^2   x^3   x^4
'''
A = np.mat('1 1; 1 0')
n = 30
print(A ** n)
```

### 通用函数

#### 加法通用函数

```python
np.add(a, b)    # 与 a+b 相同
np.add.reduce(a)  # a数组元素的累加和
np.add.accumulate(a)  # 返回累加和的过程
np.add.outer([10,20,30], a) # 求外和
```

#### 乘除法相关通用函数

```python
np.prod(a)		# 数组元素的累乘
np.cumprod(a)   # 数组元素累乘的过程
np.divide(a, b) # a/b  np.true_divide(..)
np.floor_divide(a, b)  # 地板除
np.ceil(a / b) # 天花板取整
np.trunc(a / b) # 截断取整
np.round(a / b) # 四舍五入
```

#### 位运算通用函数

**位异或**

```
c = a ^ b
c = a.__xor__(b)
c = np.bitwise_xor(a, b)
```

通过位异或可以很方便的判断两个数是否同号。

```

10   00000000 00000000 00000000 00001010
8    00000000 00000000 00000000 00001000

-8   1000
-7   1001
-6   1010
-5   1011
-4   1100
-3   1101
-2   1110
-1   1111
0    0000
1    0001
2    0010
3    0011
4    0100
5    0101
6    0110
7    0111
```

```python
# 判断两个数据是否同号
a = np.array([0, -1, 2, -3, 4, -5])
b = np.array([0,  1, 2,  3, 4,  5])
print(a)
print(b)
c = (a ^ b)
print(c)
print(np.where(c < 0)[0])
```

**位与**

```python
e = a & b
e = np.bitwise_and(a, b)
```

利用位与运算计算某个数字是否是2的幂。

```python
'''
1   2^0  00001    0   00000
2   2^1  00010    1   00001
4   2^2  00100    3   00011
8   2^3  01000    7   00111
16  2^4  10000   15   01111
'''
```

```python
# 位与运算 判断数字是否是2的幂
a = np.arange(1, 100)
c = np.bitwise_and(a, (a - 1))
print(a[c == 0])
```

其他位运算相关API：

```python
# 位或
np.bitwise_or(a, b)
# 位反
np.bitwise_not(a)
# 移位
np.left_shift(a, 1)  # a的左移1位2进制
np.right_shift(a, 1) # a的右移1位2进制
```

### 特征值与特征向量

对于n阶方阵A，如果存在数a和非零n维列向量x， 使得Ax=ax， 则称a是矩阵A的一个特征值，x是矩阵A属于特征值a的特征向量。

```python
# 特征值提取
# A：原方阵
# eigvals：提取操作后得到的特征值数组
# eigvecs：提取操作后得到的特征向量数组
eigvals, eigvecs = np.linalg.eig(A)

# 通过特征值与特征向量求取原方阵
S = np.mat(eigvecs) * 
	np.mat(np.diag(eigvals)) * 
	np.mat(eigvecs.I)
```

案例：

















