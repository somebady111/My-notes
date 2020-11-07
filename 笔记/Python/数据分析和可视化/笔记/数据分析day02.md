# 数据分析DAY02

[TOC]

## matplotlib概述

matplotlib是python的一个强大的绘图库。 

## matplotlib的基本功能

1. 基本绘图
   1. 设置线型， 线宽， 颜色
   2. 设置坐标轴范围、刻度等
   3. 设置图例
   4. 绘制特殊点
   5. 添加备注
2. 高级绘图
   1. 绘制子图
   2. 刻度定位器，刻度网格线
   3. 半对数坐标系
   4. 散点图
   5. 填充
   6. 条形图
   7. 饼状图
   8. 3D图像：线框图、曲面图、散点图。
   9. 等高线图、热成像图
   10. 简单动画



## matplotlib绘图功能详解

### 基本绘图

绘图的核心API：

```python
import matplotlib.pyplot as mp
# x: 所有点的x坐标数组   y:所有点的y坐标数组
mp.plot(x, y)
mp.show()
```

绘制水平线与垂直线：

 ```python
# 垂直线
mp.vlines(val, ymin, ymax)
# 水平线
mp.hlines(val, xmin, xmax)
 ```

案例：

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo01_basic.py 基本绘图
"""
import numpy as np
import matplotlib.pyplot as mp

a = np.array([91, 82, 82, 35, 42, 81, 95, 4])
mp.plot(a)

# 绘制水平线,位置，长度
mp.hlines([60, 50], 0, 7)
mp.vlines(4, 0, 100)

mp.show()
```

#### 线型、线宽、颜色

```python
# linestyle:线型  '-'  '--'  ':'
# linewidth:线宽  1/2/3/...
# color:颜色
#    常见的英文单词或首字母: 'red'
#    '#abcdab'
#    (0.8, 0.8, 1) 或 (1, 1, 1, 0.5) alpha透明度
mp.plot(x, y, linestyle='', linewidth=1, 
       color='', alpha=0.5)
```

案例：绘制一条正弦曲线

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo02_sin.py 绘制正弦曲线
"""
import numpy as np
import matplotlib.pyplot as mp
# 整理数据  正弦曲线
x = np.linspace(-np.pi, np.pi, 1000)
sinx = np.sin(x)
# 绘制余弦曲线 y=1/2 cos(x)
cosx = np.cos(x) / 2
# 绘图
mp.plot(x, sinx, linestyle='--', linewidth=2,
        color='dodgerblue', alpha=0.8)
mp.plot(x, cosx, linestyle=':', linewidth=3,
        color='orangered', alpha=0.8)

mp.show()
```

#### 设置坐标范围	

```python
# 设置x轴的范围  区间：[x_lim_min, x_lim_max]
mp.xlim(x_lim_min, x_lim_max)
# 设置y轴的范围  区间：[y_lim_min, y_lim_max]
mp.ylim(y_lim_min, y_lim_max)
```

#### 设置坐标刻度

```python
# 设置x轴的刻度
mp.xticks(x_val_list, x_text_list)
# 设置y轴的刻度
mp.yticks(y_val_list, y_text_list)
```

案例：

```python
# 设置坐标刻度
x_val_list = [-np.pi, -np.pi / 2, 0, np.pi / 2, np.pi]
x_text_list = [r'$-\pi$', r'$-\frac{\pi}{2}$','0', r'$\frac{\pi}{2}$', r'$\pi$']
mp.xticks(x_val_list, x_text_list)
y_val_list = [-1, -0.5, 0, 0.5, 1]
y_text_list = ['-1.0', '-0.5', '0.0', '0.5', '1.0']
mp.yticks(y_val_list, y_text_list)
```

***latex排版语法字符串***
$$
a^2+b^2=c^2
$$

#### 设置坐标轴

```python
# 获取当前坐标轴
ax = mp.gca()
# 获取某个坐标轴
axis = ax.spines['left|right|top|bottom']
# 设置位置
axis.set_position(('data', val))
# 设置颜色  none：去掉坐标轴
axis.set_color('none')
```

案例：

```python
# 设置坐标轴的位置与颜色
ax = mp.gca()
ax.spines['top'].set_color('none')
ax.spines['right'].set_color('none')
ax.spines['left'].set_position(('data', 0))
ax.spines['bottom'].set_position(('data', 0))
```

#### 绘制图例

```python
mp.plot(x, y, ...., label=r'$$')
mp.legend()
```

案例：

```python
# 绘图
mp.plot(x, sinx, linestyle='--', linewidth=2,
        color='dodgerblue', alpha=0.8,
        label='f(x)=sin(x)')
mp.plot(x, cosx, linestyle=':', linewidth=3,
        color='orangered', alpha=0.8,
        label=r'$f(x)=\frac{1}{2}cos(x)$')
```

#### 绘制特殊点

```python
mp.scatter(
    xarray, yarray, # x与y的坐标数组
    marker='',  # 点型 'D' 's' 'o' ...
    s=60,		# 点的大小
    color='',
    zorder=3    # 绘制点所有使用的图层编号, 编号越大，图层越靠上
)
```

案例：

```python
# 绘制特殊点  edgecolor边缘色  facecolor填充色
mp.scatter([np.pi / 2, np.pi / 2], [1, 0],marker='D', color='red', s=50,zorder=3)
```

#### 备注

```python
mp.annotate(
	  r'$$', #备注文本
    xycoords='data',    # 定义目标点位置
    xy=(x, y),
    textcoords='offset points',#定义文本位置
    xytext=(x, y),
    fontsize=12,    	#字体大小
    arrowprops=dict()	#箭头样式
)
arrowprops=dict(
	arrowstyle='->',		# 箭头样式
    connectionstyle='angle3'# 连接线样式
)
```

案例：

```python
mp.annotate(
    r'$[\frac{\pi}{2}, 0]$',
    xycoords='data',
    xy=(np.pi / 2, 0),
    textcoords='offset points',
    xytext=(-50, -50),
    fontsize=12,
    arrowprops=dict(
        arrowstyle='-|>',  # '-['  '-|>'
        connectionstyle='angle3')  # bar  arc
)
```

### 高级绘图

#### 绘制窗口与子图

```python
mp.figure('titleA', facecolor='gray')
mp.plot()  # 作用于A窗口
mp.figure('titleB', facecolor='gray')
mp.plot()  # 作用于B窗口
mp.figure('titleA')  # 把A窗口置为当前窗口
mp.plot()  # 作用于A窗口
mp.show()
```

常见的窗口相关参数：

```python
# 图表标题
mp.title('title', fontsize=12)
# 设置水平、垂直轴的文本
mp.xlabel('xx', fontsize=12)   
mp.ylabel('yy', fontsize=12)
# 设置刻度文本的大小
mp.tick_params(labelsize=10)
# 设置图表的网格线
mp.grid(linestyle=':')
mp.tight_layout()  # 紧凑布局
```

**子图**

**矩阵式布局**

```python
mp.figure('...')
mp.subplot(row, col, num)
mp.plot()
mp.show()

# 示例  绘制2行3列的第一幅子图  
mp.subplot(2, 3, 1)
# 绘制3行3列的第五幅子图
mp.subplot(3, 3, 5)
```

案例：	                    

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo04_matlayout.py   矩阵式子图布局
"""
import matplotlib.pyplot as mp

mp.figure('MatLayout', facecolor='lightgray')

for i in range(9):
    mp.subplot(3, 3, i + 1)
    mp.text(0.5, 0.5, i + 1, ha='center',
            va='center', size=36, alpha=0.5)
    mp.xticks([])
    mp.yticks([])

mp.tight_layout()
mp.show()			
```

**网格式布局**

网格式布局支持单元格的合并。

```python
import matplotlib.gridspec as mg
mp.figure(...)
# 构建3行3列的网格对象
gs = mg.GridSpec(3, 3)
mp.subplot(gs[0, :2])
mp.xxxx()
mp.show()
```

**自由布局**

```python
mp.figure()
# 构建一个图表  (0.1, 0.2)为图表的左下角位置 窗口高度的三分之一，五分之一
mp.axes([0.1, 0.2, 0.5, 0.3])
mp.text()
mp.show()
```

#### 刻度定位器

````python
# 获取坐标轴
ax = mp.gca()
# 定义刻度定位器对象 每隔10一个刻度值
l = mp.MultipleLocator(10)
# 设置水平坐标轴的主刻度定位器对象
ax.xaxis.set_major_locator(l)
# 设置水平坐标轴的次刻度定位器对象
ax.xaxis.set_minor_locator(l)
````

案例：绘制一个数轴

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo07_locator.py  刻度定位器
"""
import matplotlib.pyplot as mp

locators = ['mp.NullLocator()',
            'mp.MultipleLocator(1)',
            'mp.MaxNLocator(nbins=4)']

mp.figure('Locators', facecolor='lightgray')

for i, locator in enumerate(locators):
    mp.subplot(len(locators), 1, i + 1)
    ax = mp.gca()
    ax.spines['left'].set_color('none')
    ax.spines['top'].set_color('none')
    ax.spines['right'].set_color('none')
    mp.xlim(0, 10)
    mp.ylim(-1, 1)
    mp.yticks([])
    # 把水平轴移到data坐标系中0的位置
    ax.spines['bottom'].set_position(('data', 0))
    ma_loc = mp.MultipleLocator(1)
    ax.xaxis.set_major_locator(eval(locator))  # 主刻度 隔1
    mi_loc = mp.MultipleLocator(0.1)
    ax.xaxis.set_minor_locator(mi_loc)  # 次刻度 隔0.1

mp.show()
```

#### 刻度网格线

matplotlib支持沿着主刻度或次刻度绘制刻度网格线。

```python
ax = mp.gca()
ax.grid(
	which='',  # 'major|minor|both'
    axis='',   # 'x|y|both'
    linewidth=1,
    linestyle='',
    color=''
)
```

案例：

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo08_grid.py  刻度网格线
"""
import numpy as np
import matplotlib.pyplot as mp

y = np.array([1, 10, 100, 1000, 100, 10, 1])
mp.figure('Grid Line', facecolor='lightgray')
mp.title('Grid Line', fontsize=14)
mp.xlabel('X', fontsize=12)
mp.ylabel('Y', fontsize=12)
ax = mp.gca()
ax.xaxis.set_major_locator(mp.MultipleLocator(1))
ax.xaxis.set_minor_locator(mp.MultipleLocator(0.1))
ax.yaxis.set_major_locator(mp.MultipleLocator(250))
ax.yaxis.set_minor_locator(mp.MultipleLocator(50))
mp.tick_params(labelsize=10)
# 绘制刻度网格线
ax.grid(which='major', axis='both',
        linewidth=0.75, color='orange')
ax.grid(which='minor', axis='both',
        linewidth=0.25, color='orange')


mp.plot(y, color='dodgerblue', label='Line')
mp.legend()
mp.show()
```

#### 半对数坐标

半对数坐标的y轴将会以指数方式递增。如何设置当前坐标系为半对数坐标系：

```python
mp.plot() 
修改为：
mp.semilogy()
```

由于绘图过程中，可能会遇到特殊值导致底部数据的细节无法完整的观察，使用半对数坐标系可以用于放大图像中底部数据的细节。

#### 散点图

可以通过散点图中每个点的坐标、颜色、大小、形状表示样本不同的特征值。

```python
mp.scatter(
    xarray, yarray, # x与y的坐标数组
    marker='',  # 点型 'D' 's' 'o' ...
    s=60,		# 点的大小
    edgecolor='',  # 边缘色
    facecolor='',  # 填充色
    color='',
    zorder=3    # 绘制点所有使用的图层编号, 编号越大，图层越靠上
)
```

```python
def main():
    '''
    散点图
    :return:
    '''
    # 有数据
    n = 500
    x = np.random.normal(175, 20, n)
    y = np.random.normal(60, 20, n)
    # 画图
    mp.figure('Persons', facecolor='red')
    mp.title('Person', fontsize=12)
    mp.xlabel('Height', fontsize=12)
    mp.ylabel('Weight', fontsize=12)
    mp.tick_params(labelsize=0)
    d = (x - 175) ** 2 + (y - 60) ** 2
    mp.scatter(x, y, s=50, c=d, label='person points', marker='o', cmap='jet_r')
    mp.legend()
    mp.show()


if __name__ == "__main__":
    main()
```

随机数相关API：

```python
# 生成符合正态分布的一组数据
# 172： 期望
# 20：  标准差
# 500： 数组元素的个数
x = np.random.normal(172, 20, 500)
```

案例：

```python

```

#### 随机数的相关API

```python
#生成符合正太分布的一组数据
#172：期望
#20：标准差
#500：数组元素的个数
x = np.random.normal(172,20,500)
```









