# 数据分析DAY05

[TOC]

#### 布林带

布林带由三条线组成：

中轨：移动均线

上轨：中轨+2*5日收盘价标准差

下轨：中轨-2*5日收盘价标准差

由此3条线组成的带状区域即为布林带。布林带收窄代表股价趋于稳定，布林带张开代表有较大的波动空间。

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo01_bld.py  布林带
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

# 绘制收盘价的折线图
mp.figure('Closing Prices', facecolor='lightgray')
mp.title('Closing Prices', fontsize=14)
mp.xlabel('Date', fontsize=12)
mp.ylabel('Closing Prices', fontsize=12)
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
mp.plot(dates, closing_prices, c='dodgerblue',
        linewidth=2, linestyle='--',
        label='Closing Prices', alpha=0.5)


# 加权卷积的5日均线
kernel = np.exp(np.linspace(-1, 0, 5))[::-1]
kernel /= kernel.sum()
sma53 = np.convolve(closing_prices, kernel, 'valid')
mp.plot(dates[4:], sma53, c='red',
        label='SMA-53')

# 计算布林带的上轨与下轨
stds = np.zeros(closing_prices.size - 4)
for i in range(stds.size):
    stds[i] = closing_prices[i:i + 5].std()

uppers = sma53 + 2 * stds
lowers = sma53 - 2 * stds
mp.plot(dates[4:], uppers, c='steelblue',
        label='Uppers')
mp.plot(dates[4:], lowers, c='steelblue',
        label='Lowers')
mp.fill_between(
    dates[4:], uppers, lowers, uppers > lowers,
    color='dodgerblue', alpha=0.3)

mp.legend()
mp.gcf().autofmt_xdate()
mp.tight_layout()
mp.show()
```

#### 线性预测

假设一组样本数据符合一种线性规律（符合线性方程），那么就可以通过自变量预测未来将会出现的数据。

```
[a, b, c, d, e, f, ?]
[a, b, c, d, e, f, g, h, ?]

输入：
ax + by + cz = d
bx + cy + dz = e
cx + dy + ez = f
预测输出：
dx + ey + fz = ?
```

基于矩阵的方式求解方程组：
$$
\left[\begin{array}{c}
a & b & c \\
b & c & d \\
c & d & e \\
\end{array}\right]
\times
\left[\begin{array}{c}
x \\
y \\
z \\
\end{array}\right]
=
\left[\begin{array}{c}
d \\
e \\
f \\
\end{array}\right]
$$

numpy提供了线性代数模块linalg方便的求解x，y，z：

```python
# A： 3*3的矩阵
# B： 等号右边的列向量
r = np.linalg.lstsq(A, B)[0]
```

案例：假设AAPL符合上述线性规律，绘制股价预测线。

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo02_lp.py  股价的线性预测
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

# 绘制收盘价的折线图
mp.figure('Linear Predict', facecolor='lightgray')
mp.title('Linear Predict', fontsize=14)
mp.xlabel('Date', fontsize=12)
mp.ylabel('Closing Prices', fontsize=12)
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
mp.plot(dates, closing_prices, c='dodgerblue',
        linewidth=2, linestyle='--',
        label='Closing Prices', alpha=0.7)


# 绘制股价预测线 假设股价符合N元线性关系
N = 5
# pred_prices存储每个股价预测值
pred_prices = np.zeros(
    closing_prices.size - 2 * N + 1)
for i in range(pred_prices.size):
    # A为N*N的矩阵
    A = np.zeros((N, N))
    for j in range(N):
        # 整理A矩阵
        A[j, ] = closing_prices[j + i:i + j + N]
    # 整理B矩阵
    B = closing_prices[i + N:i + N * 2]
    x = np.linalg.lstsq(A, B)[0]
    pred_price = B.dot(x)
    pred_prices[i] = pred_price
# 绘制预测线
mp.plot(dates[2 * N:], pred_prices[:-1], 'o-',
        c='orangered', label='Predict Line')

mp.legend()
mp.gcf().autofmt_xdate()
mp.tight_layout()
mp.show()
```

#### 线性拟合

线性拟合可以寻求与一组散点走向趋势规律相适应的线性表达式方程。

```
[x1, y1], [x2, y2], [x3, y3], [x4, y4]
...
[xn, yn]
```

```
kx1 + b = y1
kx2 + b = y2
kx3 + b = y3
...
kxn + b = yn
```

$$
\left[\begin{array}{c}
x_1 & 1\\
x_2 & 1\\
x_3 & 1\\
x_n & 1\\
\end{array}\right]
\times
\left[\begin{array}{c}
k \\
b \\
\end{array}\right]
=
\left[\begin{array}{c}
y1 \\
y2 \\
y3 \\
yn \\
\end{array}\right]
$$

x = np.linalg.lstsq(A, B) 可以基于最小二乘法求取误差最小的一组k与b。

案例：利用线性拟合画出股价的趋势线。

趋势线：(最高价+最低价+收盘价)/3

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo03_lstsq.py  线性拟合
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

# 绘制收盘价的折线图
mp.figure('Linear Predict', facecolor='lightgray')
mp.title('Linear Predict', fontsize=14)
mp.xlabel('Date', fontsize=12)
mp.ylabel('Closing Prices', fontsize=12)
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
mp.plot(dates, closing_prices, c='dodgerblue',
        linewidth=2, linestyle='--',
        label='Closing Prices', alpha=0.3)

# 整理AAPL每天的趋势点价格
trend_points = (highest_prices + lowest_prices +
                closing_prices) / 3
mp.scatter(dates, trend_points, s=60,
           color='orangered', zorder=3,
           label='Trend Points', alpha=0.3)
# 整理A与B矩阵，完成线性拟合，求得线性方程的k与b
days = dates.astype('M8[D]').astype('int32')
A = np.column_stack((days, np.ones_like(days)))
B = trend_points
x = np.linalg.lstsq(A, B)[0]
# 绘制趋势线
trend_line = x[0] * days + x[1]
mp.plot(dates, trend_line, c='orangered',
        linewidth=2, label='Trend Line')

# 绘制顶部压力线
spreads = highest_prices - lowest_prices
upper_line = trend_line + spreads
x = np.linalg.lstsq(A, upper_line)[0]
upper_line = x[0] * days + x[1]

# 绘制底部支撑线
lower_line = trend_line - spreads
x = np.linalg.lstsq(A, lower_line)[0]
lower_line = x[0] * days + x[1]

mp.plot(dates, upper_line, c='red',
        linewidth=2, label='Upper Line')
mp.plot(dates, lower_line, c='red',
        linewidth=2, label='Lower Line')

mp.legend()
mp.gcf().autofmt_xdate()
mp.tight_layout()
mp.show()
```

#### 数组的裁剪、压缩

**数组的裁剪**

```python
# 对数组进行裁剪
# 使得数组的最小值不小于min，最大值不大于max
r = ndarray.clip(min=下限, max=上限)
```

**数组的压缩**

```python
# 返回a数组中满足条件的元素组成的新数组
b = a.compress(条件)
```

示例：

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo04_clip.py 数组的裁剪
"""
import numpy as np

a = np.arange(1, 11)
print(a)
b = a.clip(min=3, max=7)
print(b)

# 返回a数组中满足条件的元素组成的新数组
print(a)
# c = a.compress((a % 2 == 0) or (a % 3 == 0))
c = a.compress(
    np.all([a % 2 == 0, a % 3 == 0], axis=0))

print(c)
```



#### 协方差、相关矩阵、相关系数

通过两组统计数据计算而得的协方差可以评估这两组数据的相似程度。

样本：

```
A = [a1, a2, a3 .. an]
B = [b1, b2, b3 .. bn]
```

均值：

```
ave_a = np.mean(A)
ave_b = np.mean(B)
```

离差：

```
dev_a = A - ave_a
dev_b = B - ave_b
```

协方差：

```
cov_ab = (dev_a * dev_b).mean()
cov_ba = (dev_b * dev_a).mean()
```

协方差可以`简单的反映出两组统计样本的相关性`。值为正，则为正相关；若值为负，则为负相关。绝对值越大则相关性越强。

案例：统计bhp、vale两支股票的相关性。

```python
# 计算两组数据的协方差
ave_bhp = np.mean(bhp_closing_prices)
ave_vale = np.mean(vale_closing_prices)
# 离差
dev_bhp = bhp_closing_prices - ave_bhp
dev_vale = vale_closing_prices - ave_vale
# 协方差
print(np.mean(dev_bhp * dev_vale))
```

**相关系数**

协方差除以两组统计样本标准差之积  是一个[-1,1]的数。该结果称为两组统计样本的相关系数。

```
若相关系数越接近1， 两组样本越正相关。
若相关系数越接近-1， 两组样本越负相关。
若相关系数越接近0， 两组样本没啥关系。
```

```python
# 协方差
cov_ab = np.mean(dev_bhp * dev_vale)
print(cov_ab)
# 计算两组数据的相关系数
print(cov_ab / (np.std(bhp_closing_prices) *
                np.std(vale_closing_prices)))
```

**相关矩阵**

numpy提供了方法获取相关矩阵：

```python
m = np.corrcoef(a, b)
```

相关矩阵中存储了a与b两组数据的相关系数。
$$
\left[\begin{array}{c}
a与a的相关系数 & a与b的相关系数\\
b与a的相关系数 & b与b的相关系数 \\
\end{array}\right]
$$

$$
\left[\begin{array}{c}
1 & \frac{cov(ab)}{std(a)*std(b)}\\
\frac{cov(ba)}{std(b)*std(a)} & 1\\
\end{array}\right]
$$

获取a与b两组数据的协方差矩阵：

```python
m = np.cov(a, b)
```

#### 多项式拟合

多项式的一般形式：
$$
y=p_0x^n+p_1x^{n-1}+p_2x^{n-2}+... p_n
$$
在numpy的世界中，任何一元多项式都可以由一组p<sub>0</sub>~p<sub>n</sub> 系数数组来表示。

**多项式拟合的过程**

假设拟合最终得到的多项式如下：
$$
f(x)=p_0x^n+p_1x^{n-1}+p_2x^{n-2}+... p_n
$$
则拟合函数与真实结果的误差方如下：
$$
loss = (y_1-f(x_1))^2 + (y_2-f(x_2))^2 + ... +  (y_n-f(x_n))^2
$$
那么多项式拟合的目的是为了求得一组p<sub>0</sub>~p<sub>n</sub> 使得loss值取最小。

numpy提供了一系列多项式操作相关的API：

```python
# P：多项式方程的系数数组
# X：自变量 
# 把自变量代入多项式方程，返回函数值。
Y = np.polyval(P, X) 

# 多项式求导   P：原多项式的系数数组
# Q：导函数的系数数组
Q = np.polyder(P)

# 求多项式的根    当y=0时，x的取值
xs = np.roots(P)

# 多项式拟合 给出所有样本数据X，Y
P = np.ployfit(X, Y, 最高次数)
```

案例：

求多项式曲线驻点的坐标。

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo06_poly.py 多项式操作
"""
import numpy as np
import matplotlib.pyplot as mp

x = np.linspace(-20, 20, 1000)
y = 4 * x**3 + 3 * x**2 - 1000 * x + 1
# 多项式求导
P = [4, 3, -1000, 1]
Q = np.polyder(P)
# 求导函数的根
xs = np.roots(Q)
ys = np.polyval(P, xs)
mp.plot(x, y)
mp.scatter(xs, ys, marker='D', zorder=3,
           color='red', s=50)
mp.show()
```

案例：使用多项式函数拟合两支股票的差价样本数据。

```python
# 计算两支股票的差价
diff_prices = bhp_closing_prices - vale_closing_prices

mp.plot(dates, diff_prices, c='dodgerblue',
        linewidth=1, linestyle='--',
        label='Diff Princes', alpha=0.4)
mp.scatter(dates, diff_prices, s=50,
           c='steelblue')

# 针对差价函数，执行多项式拟合
days = dates.astype('M8[D]').astype('int32')
P = np.polyfit(days, diff_prices, 4)
polyline = np.polyval(P, days)
mp.plot(dates, polyline, c='orangered',
        linewidth=2, linestyle='-',
        label='PolyLine')
```

#### 数据平滑

数据平滑处理通常包含有降噪、拟合等操作。降噪的功能在于去除额外的影响因素，拟合的目的在于数学模型化，从而可以通过更多的数学工具方法识别曲线的特征信息。








