# 数据分析DAY04

[TOC]

#### 绘制K线图

绘制蜡烛图

```python

# 绘制每一天的蜡烛图
rise = closing_prices <= opening_prices
# 整理填充色
color = np.array(
    [('white' if x else 'green')
     for x in rise])
# 整理边缘色
ecolor = np.array(
    [('red' if x else 'green')
     for x in rise])

# 绘制影线
mp.vlines(dates, lowest_prices, highest_prices,
          color=ecolor)
# 绘制实体
mp.bar(dates, closing_prices - opening_prices,
       0.8, opening_prices, zorder=3,
       color=color, edgecolor=ecolor)

mp.legend()
```

#### 算数平均值

算数平均值用来对一组数据进行真值的无偏估计。

```
s = [s1, s2, s3 ..., sn]
m = (s1+s2+..+sn)/n
```

numpy提供均值相关API：

```python
m = np.mean(s)
m = s.mean()
```

#### 加权平均值

一组数据中的每个元素对真值的估计重要程度不同，所以可以为每个元素分配一个权重，计算加权平均值。

```python
s = [s1, s2, s3 ..., sn]
w = [w1, w2, w3 ..., wn]
a = (s1w1 + s2*w2 + ..+snwn)/(w1+w2+..wn)
```

```python
# 求加权平均
a = np.average(s, weights=w)
```

VWAP - 交易量加权平均价格

成交量体现了市场对当前交易价格的认可程度， 成交量加权平均价格将会更加接近这支股票的真值。

```python
# 求出交易量加权均线 VWAP
vwap = np.average(closing_prices, weights=volumns)
mp.hlines(vwap, dates[0], dates[-1],
          color='steelblue', label='VWAP')
```

TWAP - 时间加权平均价格

```python
tw = np.arange(1, 31)
twap = np.average(closing_prices, weights=tw)
mp.hlines(twap, dates[0], dates[-1],
          color='violet', label='TWAP')
```

#### 最值

```python
np.max()	# 最大值  
np.min()	# 最小值
np.ptp()	# 求一组数据的极差  max-min
```

```python
np.argmax()	# 返回最大值的索引
np.argmin() # 返回最小值的索引
```

```python
np.maximum(a,b)	# 将两同维数组比较，保留较大值
np.minimum(a,b)	# 将两同维数组比较，保留较小值
```

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo03_max.py  最值
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
    lowest_prices, closing_prices, volumns = np.loadtxt(
        '../da_data/aapl.csv', unpack=True,
        delimiter=',', usecols=(1, 3, 4, 5, 6, 7),
        dtype='M8[D], f8, f8, f8, f8, f8',
        converters={1: dmy2ymd})

# 评估AAPL股价波动性
max_v = np.max(highest_prices)
min_v = np.min(lowest_prices)
print(min_v, '~', max_v)

# 查看AAPL最大、小值的日期，分析为什么会出现这种情况
max_i = np.argmax(highest_prices)
min_i = np.argmin(lowest_prices)
print('max date:', dates[max_i])
print('min date:', dates[min_i])

a = np.arange(1, 10).reshape(3, 3)
b = a.ravel()[::-1].reshape(3, 3)
print(a, b, sep='\n')
print(np.maximum(a, b))
print(np.minimum(a, b))
```

#### 中位数

将多个样本按照大小排序，取中间位置的元素。

```python
# s数组为有序数组
m = np.median(s)
```

```python
# 绘制中位数水平线
closing_prices = np.msort(closing_prices)
median = np.median(closing_prices)
mp.hlines(median, dates[0], dates[-1],
          color='green', label='Median')
```

#### 标准差

```
样本：S = [s1, s2, s3.., sn]
均值：m = np.mean(S)
离差：D = [d1, d2, d3.., dn] di=Si-m
离差方：Q = [q1, q2 ... qn]  qi=di^2
总体方差： v = (q1+q2+..qn)/n
总体标准差： s = np.sqrt(v)  

样本方差： v' = (q1+q2+..qn)/(n-1)
样本标准差： s' = np.sqrt(v')  
```

标准差相关API：

```python
std=np.std(S)   # 总体标准差
std=np.std(S, ddof=1) # 计算方差的分母为n-1
```

### 案例应用

#### 时间数据处理

案例：统计每个周一、周二......周五的收盘价的平均值。

```python
# wdays：0:周一, 1:周二, .... 4:周五
# 把算出的均值，存入数组
avg_prices = np.zeros(5)
for wday in range(avg_prices.size):
    avg_prices[wday] = \
        closing_prices[wdays == wday].mean()

# zip方法，双列表遍历
for wday, avg_price in zip(
        ['Mon', 'Tue', 'Wed', 'Thu', 'Fri'],
        avg_prices):
    print(wday, ':', avg_price)
```

#### 数据的轴向汇总

numpy提供了轴向汇总的相关API，可以方便的统计二维数据结构中以行或列为单位的数据。

```python
def func(data):
    pass
# 沿着axis定义的轴向（0, 1），把array数组中的                                                                                                  每一行（或每一列）交给func函数进行汇总处理。
np.apply_along_axis(func, axis, array)
```

基于轴向汇总，统计每周一...周五的最高、最低及均价。

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo07_axis.py  轴向汇总
"""
import numpy as np
import matplotlib.pyplot as mp
import datetime as dt
import matplotlib.dates as md


def dmy2wday(s):
    dmy = str(s, encoding='utf-8')
    time = dt.datetime.strptime(dmy, '%d-%m-%Y')
    # 返回日期对象的星期数
    wday = time.date().weekday()
    return wday

# 读取文件数据
wdays, closing_prices = np.loadtxt(
    '../da_data/aapl.csv', unpack=True,
    delimiter=',', usecols=(1, 6),
    dtype='f8, f8',
    converters={1: dmy2wday})

# 基于轴向汇总，统计每周一...周五的最高、最低及均价。
indices = np.arange(closing_prices.size)
mon_i = indices[wdays == 0]
tue_i = indices[wdays == 1]
wed_i = indices[wdays == 2]
thu_i = indices[wdays == 3]
fri_i = indices[wdays == 4]
print(mon_i)
print(tue_i)
print(wed_i)
print(thu_i)
print(fri_i)
# 把缺失的值补全
# 为mon_i数组进行补全
# 首位补0个数，末尾补2个数  补充数字的值为0
mon_i = np.pad(mon_i, pad_width=(0, 2),
               mode='constant', constant_values=-1)
tue_i = np.pad(tue_i, pad_width=(0, 1),
               mode='constant', constant_values=-1)
wed_i = np.pad(wed_i, pad_width=(0, 1),
               mode='constant', constant_values=-1)
thu_i = np.pad(thu_i, pad_width=(0, 1),
               mode='constant', constant_values=-1)
# 整理二维数组
data = np.array([mon_i, tue_i, wed_i, thu_i, fri_i])
print(data)


def func(row):
    row = row[row != -1]
    return closing_prices[row].max(), \
        closing_prices[row].min(),  \
        closing_prices[row].mean()

# 轴向汇总
rep = np.apply_along_axis(func, 1, data)
print(rep)
```

#### 移动均线

收盘价5日移动均线：从第5天开始，每天计算最近5天的收盘价的平均值所构成的一条线。

```
(a+b+c+d+e)/5
(b+c+d+e+f)/5
(c+d+e+f+g)/5
...
```

案例：

```python
# 绘制5日均线图
sma5 = np.zeros(closing_prices.size - 4)
for i in range(sma5.size):
    sma5[i] = closing_prices[i:i + 5].mean()
mp.plot(dates[4:], sma5, c='orangered',
        label='SMA-5')
```

#### 卷积

卷积运算的运算规则：

```
a = [1, 2, 3, 4, 5]    原始数组
b = [8, 7, 6]          卷积核数组  kernel

使用b作为卷积核，对a执行卷积运算的过程如下：

           44 65 86         有效卷积 valid
        23 44 65 86 59      同维卷积 same
      8 23 44 65 86 59 30   完全卷积 full
0  0  1  2  3  4  5  0  0
6  7  8
   6  7  8
      6  7  8
         6  7  8
            6  7  8
               6  7  8
                  6  7  8
```

```
基于卷积可以实现5日均线业务
[a, b, c, d, e, f, g, h...]
[1/5, 1/5, 1/5, 1/5, 1/5]
```

numpy提供了卷积运算相关API：

```python
# array： 原数组
# kernel： 卷积核数组
# valid： 获取有效卷积运算结果
r = np.convolve(array, kernel, 'valid')
```

案例：基于卷积运算，得到5日均线

```python
# 使用卷积得到10日均线
kernel = np.ones(10) / 10
sma10 = np.convolve(closing_prices, kernel, 'valid')
mp.plot(dates[9:], sma10, c='violet',
        linewidth=2,  label='SMA-10')
```

##### 加权卷积

卷积运算过程中，每一个价格都应对应一个权重，时间越靠后的价格求均值时的权重应越高。

```python
# 加权卷积的5日均线
kernel = np.exp(np.linspace(-1, 0, 5))[::-1]
kernel /= kernel.sum()
sma53 = np.convolve(closing_prices, kernel, 'valid')
mp.plot(dates[4:], sma53, c='red',
        label='SMA-53')
```

























