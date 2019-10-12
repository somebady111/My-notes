# 数据分析DAY03

[TOC]

### 高级绘图

#### 绘制填充

以某种颜色填充图像中的闭合区域。

```python
mp.fill_between(
	x, 
    sinx,
    cosx, 
    sinx < cosx, # 填充条件
    color='',
    alpha=0.6
)
```

案例：

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo01_fill.py  填充图
"""
import numpy as np
import matplotlib.pyplot as mp

# 生成数据
n = 1000
x = np.linspace(0, 8 * np.pi, n)
sinx = np.sin(x)
cosx = np.cos(x / 2) / 2

mp.figure('Fill', facecolor='lightgray')
mp.title('Fill', fontsize=16)
mp.xlabel('X', fontsize=12)
mp.ylabel('Y', fontsize=12)
mp.tick_params(labelsize=10)
mp.plot(x, sinx, color='dodgerblue',
        label=r'$y=sin(x)$')
mp.plot(x, cosx, color='orangered',
        label=r'$y=\frac{1}{2}cos(\frac{x}{2})$')
# 设置填充
mp.fill_between(x, sinx, cosx, sinx > cosx,
                color='dodgerblue', alpha=0.6)
mp.fill_between(x, sinx, cosx, sinx < cosx,
                color='orangered', alpha=0.6)

mp.legend()
mp.show()
```

#### 条形图(柱状图)

```python
mp.figure()
mp.bar(
	x,	# 水平坐标数组
    y,  # 每个柱子的高度数组
    width,	# 柱子的宽度
    color='',
    label=''
)
```

案例：

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo02_bar.py  条形图
"""
import numpy as np
import matplotlib.pyplot as mp

# 生成数据
apples = np.array([64, 56, 85, 79, 17, 45, 68, 94, 36, 58, 34, 91])
oranges = np.array([96, 58, 34, 58, 29, 34, 56, 23, 94, 56, 23, 56])

# 绘制图像
mp.figure('Bar', facecolor='lightgray')
mp.title('Bar', fontsize=16)
mp.xlabel('Month', fontsize=12)
mp.ylabel('Sells', fontsize=12)
mp.tick_params(labelsize=10)
x = np.arange(apples.size)
mp.bar(x - 0.2, apples, 0.4, color='dodgerblue',
       label='Apples', align='center')
mp.bar(x + 0.2, oranges, 0.4, color='orangered',
       label='Oranges')
# x坐标轴刻度
mp.xticks(x, ['Jan', 'Feb', 'Mar', 'Apr', 'May','Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
mp.legend()
mp.show()
```

#### 饼状图

```python
mp.pie(
	values, 	# 一组数据列表
    spaces, 	# 扇形之间的间距列表
    labels,		# 标签列表
    colors,		# 扇形的颜色列表
    '%.2f%%',	# 百分比的格式
    shadow=True,# 添加阴影
    starangle=90# 起始角度
)
```

案例：

````python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo03_pie.py  饼状图
"""
import numpy as np
import matplotlib.pyplot as mp

mp.figure('Pie Chart', facecolor='lightgray')
mp.title('Pie Chart', fontsize=12)
# 整理数据
values = [26, 17, 21, 29, 11]
spaces = [0.05, 0.01, 0.01, 0.01, 0.01]
labels = ['Python', 'Javascript', 'C++',
          'Java', 'PHP']
colors = ['dodgerblue', 'orangered',
          'limegreen', 'violet', 'gold']

# 绘制图形
mp.axis('equal')  # 设置等轴比例
mp.pie(values, spaces, labels, colors,
       '%.2f%%', shadow=True, startangle=45)
mp.legend()
mp.show()
````

#### 等高线图

```python
cntr=mp.contour(
	x,   # 网格点坐标矩阵的x坐标数组
    y,	 # 网格点坐标矩阵的y坐标数组
    z,   # 网格点坐标矩阵中每个坐标的高度数组
    8,   # 把整体高度分8份
    colors='black',
    linewidths=0.5   
)
# 为等高线添加高度值标签文本
mp.clabel(cntr, inline_spacing=1, fmt='%.1f',fontsize=10)
# 为等高线图绘制填充色
mp.contourf(x, y, z, 8, cmap='jet')
```

案例：

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo04_contour.py  绘制等高线
"""
import numpy as np
import matplotlib.pyplot as mp

# 整理数据
n = 1000
# x与y：从-3~3拆1000个点，返回拆分后的点集
x, y = np.meshgrid(np.linspace(-3, 3, n),
                   np.linspace(-3, 3, n))
# 通过一个数学公式计算每个网格点的高度
z = (1 - x / 2 + x**5 + y**3) * \
    np.exp(-x**2 - y**2)

# 绘制图形
mp.figure('Contour', facecolor='lightgray')
mp.title('Contour', fontsize=14)
mp.xlabel('x', fontsize=12)
mp.ylabel('y', fontsize=12)
mp.tick_params(labelsize=10)
mp.grid(linestyle=':')
# 绘制等高线图
cntr = mp.contour(x, y, z, 8, colors='black',
                  linewidths=0.5)
# 为等高线添加高度值标签文本，inline_spacing留白
mp.clabel(cntr, inline_spacing=1, fmt='%.1f',fontsize=10)
# 为等高线图绘制填充色
mp.contourf(x, y, z, 8, cmap='jet')

mp.show()
```

#### 热成像图

```python
# 把z矩阵以图像的形式显示出来
# z中每个位置都有相应的数值，根据数值大小，从
# cmap映射中选取当前位置的颜色从而绘制整个图像
mp.imshow(z, cmap='jet')
mp.colorbar()
```

案例：

```python
# 绘制图形
mp.figure('Imshow', facecolor='lightgray')
mp.title('Imshow', fontsize=14)
mp.xlabel('x', fontsize=12)
mp.ylabel('y', fontsize=12)
mp.tick_params(labelsize=10)
mp.grid(linestyle=':')
mp.imshow(z, cmap='jet', origin='lower')
mp.colorbar()
mp.show()
```

#### 3D图像的绘制

```python
from mpl_toolkits.mplot3d import axes3d
ax3d = mp.gca(projection='3d')
# 调用ax3d的方法绘制3D图形
```

**3D散点图**

```python
ax3d.scatter(
	x, y, z, 
    s=60,
    marker='',
    edgecolor='',
    facecolor='',
    c=d,
    cmap=''
)
```

案例：

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo06_3dscatter.py  3d散点图
"""
import numpy as np
import matplotlib.pyplot as mp
from mpl_toolkits.mplot3d import axes3d

# 准备数据
n = 500
x = np.random.normal(0, 1, n)
y = np.random.normal(0, 1, n)
z = np.random.normal(0, 1, n)

# 绘制图像
mp.figure('3D Points', facecolor='lightgray')
ax3d = mp.gca(projection='3d')
ax3d.set_xlabel('x', fontsize=12)
ax3d.set_ylabel('y', fontsize=12)
ax3d.set_zlabel('z', fontsize=12)
mp.tick_params(labelsize=10)
d = x**2 + y**2 + z**2
ax3d.scatter(x, y, z, c=d, cmap='jet', s=40,
             label='Points')
mp.tight_layout()
mp.show()

```

**3D线框图**

```python
ax3d.plot_wireframe(
    x, y, 			# x、y网格点坐标矩阵
    z,				# 每个坐标的z的值
	rstride=30,		# 行跨距
	cstride=30,		# 列跨距
	linewidth=1,
    color=''
)
```

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo04_contour.py  绘制等高线
"""
import numpy as np
import matplotlib.pyplot as mp
from mpl_toolkits.mplot3d import axes3d

# 整理数据
n = 1000
# x与y：从-3~3拆1000个点
x, y = np.meshgrid(np.linspace(-3, 3, n),
                   np.linspace(-3, 3, n))
# 通过一个数学公式计算每个网格点的高度
z = (1 - x / 2 + x**5 + y**3) * \
    np.exp(-x**2 - y**2)

# 绘制图形
mp.figure('Wire Frame', facecolor='lightgray')
mp.title('Wire Frame', fontsize=14)
ax3d = mp.gca(projection='3d')
ax3d.set_xlabel('x', fontsize=12)
ax3d.set_ylabel('y', fontsize=12)
ax3d.set_zlabel('z', fontsize=12)
ax3d.plot_wireframe(x,y,z,rstride=30,cstride=30,linewidth=1,color='dodgerblue')

mp.show()

```

**3D曲面图**

```python
ax3d.plot_surface(
    x, y, 			# x、y网格点坐标矩阵
    z,				# 每个坐标的z的值
	rstride=30,		# 行跨距
	cstride=30,		# 列跨距
    cmap='jet'
)
```

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo04_contour.py  绘制等高线
"""
import numpy as np
import matplotlib.pyplot as mp
from mpl_toolkits.mplot3d import axes3d

# 整理数据
n = 1000
# x与y：从-3~3拆1000个点
x, y = np.meshgrid(np.linspace(-3, 3, n),
                   np.linspace(-3, 3, n))
# 通过一个数学公式计算每个网格点的高度
z = (1 - x / 2 + x**5 + y**3) * \
    np.exp(-x**2 - y**2)

# 绘制图形
mp.figure('Wire Frame', facecolor='lightgray')
mp.title('Wire Frame', fontsize=14)
ax3d = mp.gca(projection='3d')
ax3d.set_xlabel('x', fontsize=12)
ax3d.set_ylabel('y', fontsize=12)
ax3d.set_zlabel('z', fontsize=12)
ax3d.plot_surface(x,y,z,rstride=10,cstride=10,cmap='jet')
mp.tight_layout()
mp.show()

```

#### 极坐标系

与笛卡尔坐标系不同，某些情况下极坐标系更加适合显示与角度有关系的图像。

```python
#若希望坐标系变为极坐标系，只需要设置：
mp.gca(projection='polar')
```

#### 简单动画

动画即是在一段时间内快速连续的重新绘制图像的过程。

matplotlib提供了方法用于处理简单的动画绘制。定义update函数， 每隔一段时间自动执行update函数更新界面。

```python
import matplotlib.animation as ma
# 定义更新界面的逻辑
def update(number):
    pass

# 针对当前窗口，每10毫秒执行一次update函数
ma.FuncAnimation(
    mp.gcf(), update, interval=10)
```

案例：随机生成各种颜色、大小、位置的100个气泡，并使他们不断增大。

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo11_balls.py  动画小球
"""
import numpy as np
import matplotlib.pyplot as mp
import matplotlib.animation as ma

# 整理数据
n = 100
balls = np.zeros(100, dtype=[
    ('position', float, 2),
    ('size', float, 1),
    ('growth', float, 1),
    ('color', float, 3)])

balls['position'] = np.random.uniform(0, 1, (n, 2))
balls['size'] = np.random.uniform(50, 70, n)
balls['growth'] = np.random.uniform(10, 20, n)
balls['color'] = np.random.uniform(0, 1, (n, 3))

# 绘制图像
mp.figure('Balls Animation', facecolor='lightgray')
mp.title('Balls Animation', fontsize=14)
mp.xticks([])
mp.yticks([])
sc = mp.scatter(balls['position'][:, 0],
                balls['position'][:, 1],
                s=balls['size'],
                color=balls['color'],
                alpha=0.8)


def update(number):
    balls['size'] += balls['growth']
    boom_i = number % 100
    balls[boom_i]['size'] = np.random.uniform(50, 70, 1)
    balls[boom_i]['position'] = np.random.uniform(0, 1, (1, 2))
    # 更新每个球的大小
    sc.set_sizes(balls['size'])
    sc.set_offsets(balls['position'])

anim = ma.FuncAnimation(mp.gcf(), update, interval=30)
mp.show()
```

matplotlib提供了生成器函数用于获取动态数据，拿到动态数据后自动调用update函数更新界面。

````python
def update(data):
    pass

def generator():
    yield data

ma.FuncAnimation(mp.gcf(), update, generator, interval=10)
````

案例：模拟心电图

````python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo12_anim.py  带有生成器函数的简单动画
"""
import numpy as np
import matplotlib.pyplot as mp
import matplotlib.animation as ma

mp.figure('Signal', facecolor='lightgray')
mp.title('Signal', fontsize=14)
mp.xlim(0, 10)
mp.ylim(-3, 3)
mp.grid(linestyle=':')
pl = mp.plot([], [], color='dodgerblue',
             label='Signal')[0]

x = 0


def update(data):
    t, v = data
    x, y = pl.get_data()
    x = np.append(x, t)  # x末尾追加t
    y = np.append(y, v)  # y末尾追加v
    pl.set_data(x, y)
    # 移动坐标轴
    if(x[-1] > 10):
        mp.xlim(x[-1] - 10, x[-1])


def generator():
    # 通过x  计算y， 作为一组新数据交给update
    global x
    y = np.sin(2 * np.pi * x) * \
        np.exp(np.sin(0.2 * np.pi * x))
    yield (x, y)
    x += 0.05

# 基于动画动态绘制图像
anim = ma.FuncAnimation(
    mp.gcf(), update, generator, interval=30)

mp.tight_layout()
mp.legend()
mp.show()
````

### 基于python常见的数据分析业务

#### 加载文件

```python
def func(s):
    return '2011-11-11'

a, b, c, d, e = np.loadtxt(
    '../data/a.txt', 	# 文件路径
    skiprows=1,			# 跳过头部1行
    delimiter=',', 		# 字段间的分隔符
    usecols=(1,3,4,5,6),# 读取哪些列
    unpack=True,		# 是否解包二维数组
    dtype='M8[D],f8,f8,f8,f8',
    converters={1:func}
)
```

案例：

```python
# 基于python的读取数据
import numpy as np
import matplotlib.pyplot as mp
import datetime as dt
import time
import matplotlib.dates as md

# 读取数据
def dmy2ymd(s):
    dmy = str(s,encoding='utf-8')
    time = dt.datetime.strptime(dmy,'%d-%m-%Y')
    ymd = time.date().strftime('%Y-%m-%d')
    return ymd

dates,opening_prices,highest_prices,lowest_prices,closing_prices = np.loadtxt(
    '../da_data/aapl.csv',unpack=True,delimiter=',',usecols=(1,3,4,5,6),
    dtype='M8[D],f8,f8,f8,f8',converters={1:dmy2ymd})
print(dates)

# 绘制收盘价的折线图
mp.figure('Closeing Prices',facecolor='lightgray')
mp.title('Closeing Prices',fontsize=14)
mp.xlabel('Date',fontsize=12)
mp.ylabel('Closeing Prices',fontsize=12)
mp.grid(linestyle=':')
mp.tick_params(labelsize=10)

# 设置刻度定位器
ax = mp.gca()
ax.xaxis.set_major_locator(md.WeekdayLocator(byweekday=md.MO))
ax.xaxis.set_major_formatter(md.DateFormatter('%d %b %Y'))
# 设置此刻度定位器为日定位器
ax.xaxis.set_minor_locator(md.DayLocator())


# 将dates转换为matplotlib识别的Datetime类型
dates=dates.astype(md.datetime.datetime)
mp.plot(dates,closing_prices,c='dodgerblue',linewidth=2,linestyle='--',
        label='Closeing Prices')
mp.legend()
# 自动可视化x轴的日期
mp.gcf().autofmt_xdate()
mp.tight_layout()
mp.show()
```













