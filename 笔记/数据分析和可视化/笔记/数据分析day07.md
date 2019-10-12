# 数据分析DAY07

[TOC]

#### 奇异值分解

有一个矩阵M，可以分解为3个矩阵：U，S，V。使得     U x S x V 等于M。U与V都是正交矩阵（乘以自身的转置矩阵结果为单位矩阵）。那么S矩阵主对角线上的元素称为矩阵M的奇异值，其他元素都为0。

奇异值分解可以针对非方阵，而特征值提取只能作用于方阵。

```python
U, sv, V = np.linalg.svd(M)
```

案例：

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo01_svd.py 奇异值分解
"""
import numpy as np

M = np.mat('4 11 14; 8 7 -2')
U, sv, V = np.linalg.svd(M, full_matrices=False)
print(U, U.shape)
print(sv, sv.shape)
print(V, V.shape)
print(U * U.T)
print(V * V.T)
print(np.mat(U) * np.mat(np.diag(sv)) * np.mat(V))
```

#### 快速傅里叶变换(fft)

什么是傅里叶定理？

法国科学家傅里叶说过，任何一条周期曲线，无论多么跳跃或不规则，都能表示成一组光滑的正弦曲线叠加之和。

**验证傅里叶定理**
$$
y = 4\pi \times sin(x) \\
y = \frac{4}{3}\pi \times sin(3x) \\
y = \frac{4}{5}\pi \times sin(5x) \\
...\\
y = \frac{4}{2n-1}\pi \times sin((2n-1)x) \\
$$

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo03_fft.py  傅里叶定理
"""
import numpy as np
import matplotlib.pyplot as mp

x = np.linspace(-2 * np.pi, 2 * np.pi, 1000)
y1 = 4 * np.pi * np.sin(x)
y2 = 4 / 3 * np.pi * np.sin(3 * x)
y3 = 4 / 5 * np.pi * np.sin(5 * x)
y4 = 4 / 7 * np.pi * np.sin(7 * x)
n = 1000
y = np.zeros(n)
for i in range(1, n + 1):
    y += 4 / (2 * i - 1) * np.pi * \
        np.sin((2 * i - 1) * x)

mp.plot(x, y1, label='y1', alpha=0.3)
mp.plot(x, y2, label='y2', alpha=0.3)
mp.plot(x, y3, label='y3', alpha=0.3)
mp.plot(x, y4, label='y4', alpha=0.3)
mp.plot(x, y, label='y')
mp.legend()
mp.show()
```

什么叫傅里叶变换？

我们可以基于傅里叶定理，对一个已知的曲线进行拆解，得到一组光滑的正弦曲线。这组正弦曲线的振幅、频率、相位角都不同，而这组正弦曲线叠加之后即为该已知曲线。那么对已知曲线的分解过程称为傅里叶变换。

傅里叶变换的目的是可将时域（时间域）上的信号转变为频域（频率域）上的信号，随着域的不同，对同一事物的了解角度也就随之改变。因此在时域中某些不好处理的地方，在频域就可以较为简单的处理。这就可以大量减少处理信号的复杂度。

傅里叶变换相关API：

```python
import numpy.fft as nf
# 通过采样数量与采样周期求得分解所得曲线的频率序列
freqs = nf.fftfreq(采样数量， 采样周期)
# 针对曲线做傅里叶变换获取拆解得到的所有正弦曲线
# 复数数组每个元素存储了每个正弦曲线的信息：
# 复数的模代表振幅；复数的辅角代表初相位
复数数组 = nf.fft(原函数值y)
# 逆向傅里叶变换
原函数 = np.ifft(复数数组)
```

案例：针对合成波做快速傅里叶变换，绘制频域图像。

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo04_fft.py  傅里叶变换拆解合成波
"""
import numpy as np
import matplotlib.pyplot as mp
import numpy.fft as nf

x = np.linspace(-2 * np.pi, 2 * np.pi, 1000)
y1 = 4 * np.pi * np.sin(x)
y2 = 4 / 3 * np.pi * np.sin(3 * x)
y3 = 4 / 5 * np.pi * np.sin(5 * x)
y4 = 4 / 7 * np.pi * np.sin(7 * x)
n = 1000
y = np.zeros(n)
for i in range(1, n + 1):
    y += 4 / (2 * i - 1) * np.pi * \
        np.sin((2 * i - 1) * x)

mp.figure('Signal')
mp.subplot(121)
mp.title('Time Zone', fontsize=14)
mp.plot(x, y1, label='y1', alpha=0.3)
mp.plot(x, y2, label='y2', alpha=0.3)
mp.plot(x, y3, label='y3', alpha=0.3)
mp.plot(x, y4, label='y4', alpha=0.3)
mp.plot(x, y, label='y')

# 基于傅里叶变换，拆解y
complex_ary = nf.fft(y)
y2 = nf.ifft(complex_ary)
mp.plot(x, y2.real, linewidth=7, c='red',
        alpha=0.4, label='y2')
mp.legend()

# 绘制频域图像  subplot(122)

mp.subplot(122)
# 采样频率=1000  采样周期为相邻两点间距离
freqs = nf.fftfreq(x.size, x[1] - x[0])
pows = np.abs(complex_ary)
mp.title('Frequency Zone',  fontsize=14)
mp.plot(freqs[freqs > 0], pows[freqs > 0],
        color='orangered')

mp.tight_layout()
mp.show()
```

**基于傅里叶变换的频域滤波**

含噪信号是高能信号与低能噪声叠加的信号，可以通过傅里叶变换的频域滤波实现降噪。

通过fft使含噪信号转为含噪频谱，在频域中取出低能噪声，留下高能频谱后通过IFFT生成高能信号。

**案例：** 

1. 读取音频文件，获取音频文件的基本信息：采样率，采样周期，每个采样点的信号值。绘制时域图像。

```python
sample_rates, noised_sigs =\
    wf.read('../da_data/noised.wav')
print('sample_rates:', sample_rates)
print('noised_sigs:', noised_sigs.shape)
times = np.arange(len(noised_sigs)) / sample_rates
mp.figure('Filter', facecolor='lightgray')
mp.subplot(221)
mp.title('Time Domain', fontsize=14)
mp.ylabel('Signal', fontsize=12)
mp.grid(linestyle=':')
mp.plot(times[:178], noised_sigs[:178],
        color='dodgerblue', label='Noised')
mp.legend()
```

2. 基于傅里叶变换， 获取音频频域信息，绘制音频频域的：频率/能量图像。

```python
# 基于傅里叶变换， 获取音频频域信息，
# 绘制音频频域的：频率/能量图像。
complex_noised = nf.fft(noised_sigs)
pows = np.abs(complex_noised)
freqs = nf.fftfreq(times.size, 1 / sample_rates)
mp.subplot(222)
mp.title('Frequency Domain', fontsize=14)
mp.ylabel('Powers', fontsize=12)
mp.grid(linestyle=':')
mp.semilogy(freqs[freqs > 0], pows[freqs > 0],
            color='orangered', label='Noised')
mp.legend()
```

3. 将低频噪声去除后绘制音频频域的：频率/能量图像。

```python
# 将低频噪声去除后绘制音频频域的：频率/能量图像。
fund_freq = freqs[pows.argmax()]
# 获取所有噪声频率下标组成的数组
noised_indices = np.where(freqs != fund_freq)
complex_filter = complex_noised.copy()
complex_filter[noised_indices] = 0
filter_pows = np.abs(complex_filter)
mp.subplot(224)
mp.ylabel('Powers', fontsize=12)
mp.grid(linestyle=':')
mp.plot(freqs[freqs > 0],
        filter_pows[freqs > 0],
        color='orangered', label='Filter')
mp.legend()
```

4. 基于逆向傅里叶变换，生成新的音频信号，绘制音频时域的：时间/位移图像。

```python
# 基于逆向傅里叶变换，生成新的音频信号，
# 绘制音频时域的：时间/位移图像。
filter_sigs = nf.ifft(filter_pows)
mp.subplot(223)
mp.ylabel('Signal', fontsize=12)
mp.grid(linestyle=':')
mp.plot(times[:178], filter_sigs[:178].real,
        color='dodgerblue', label='Filter')
mp.legend()
```

5. 重新生成音频文件：

```python
# 重新生成音频文件：
wf.write('../da_data/out.wav', sample_rates,
filter_sigs.real.astype(np.int16))
mp.tight_layout()
```

### 随机数模块

生成服从特定统计规律的随机数序列。

```python
np.random.normal()
np.random.uniform()
```

#### 二项分布(binomial)

二项分布就是重复n次独立事件的伯努利实验。在每次实验中只有两种可能结果，而且两种结果发生与否相互对立，并且相互独立。事件发生与否的概率在每一次独立实验中都保持不变。

```python
# 随机生成一组数据，符合二项分布
# 产生size个随机数，每个随机数来自n次尝试中成功的
# 次数，其中每次尝试成功的概率为p。
a = np.random.binomial(n, p, size)
[8,9,6,7,8,6,6,0,5,6]
```

1. 某人投篮命中率0.3， 投10次进5个球的概率。

```python
# 某人投篮命中率0.3， 投10次进5个球的概率。
a = np.random.binomial(10, 0.3, 100000)
print((a == 5).sum() / a.size)
```

#### 超几何分布

```python
# 产生size个随机数，每个随机数为在总样本中随机抽
# 取nsamle个样本后，好样本的个数。总样本由ngood
# 个好样本和nbad个坏样本组成。
a = np.random.hypergeometric(
    ngood, nbad, nsample, size)
```

案例：将25个好球与5个坏球放一起，无放回的抽出5个球，求5个球都是好球的概率。

```python
# 将25个好球与5个坏球放一起，
# 无放回的抽出5个球，求5个球都是好球的概率。
b = np.random.hypergeometric(25, 5, 5, 100000)
print((b == 5).sum() / b.size)
```

#### 正态分布

```python
# 生成size个随机数，符合标准正态分布 E=0  s=1
np.random.normal(size)
#172: 期望  
#20 : 标准差
np.random.normal(172, 20, size)
```

标准正态分布的概率密度函数：
$$
标准正态分布的概率密度函数:
\frac{e^{-\frac{x^2}{2}}}{\sqrt{2\pi}}
$$


### 杂项功能

#### 排序

```python
np.msort(a)
```

**联合间接排序**

联合间接排序支持为待排 序列排序，若待排序列值相同，则利用参考序列作为参考继续排序。最终返回排序后的索引序列。

```python
sort_indices = np.lexsort((次序列, 主序列))
```

案例：

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo07_sort.py 排序
"""
import numpy as np

pros = np.array(['Apple', 'Huawei', 'Mi', 'Oppo', 'Vivo'])
prices = np.array([8888, 4999, 2999, 3999, 3999])
vols = np.array([100, 200, 70, 80, 90])

indices = np.lexsort((-vols, prices))
print(pros[indices])
```

**插入排序**

如果有需求向有序数组中插入元素，使数组依然有序，numpy提供了searchsorted方法查询并返回可插入位置数组。

```python
indices=np.searchsorted(有序序列，待插入序列)
# 向a数组的indices指定的位置，插入b数组中的元素
c = np.insert(a, indices, b)
```

案例：

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo08_insert.py 插入排序
"""
import numpy as np

a = np.array([1, 2, 3, 5, 7, 9])
b = np.array([4, 6, 8])

indices = np.searchsorted(a, b)
print(indices)
# 向a数组的indices指定的位置，插入b数组中的元素
c = np.insert(a, indices, b)
print(c)
```

#### 插值

scipy提供了常见的插值算法可以通过一定规律拟合出插值器函数。那么可以解决离散数据连续化的需求。

```python
import scipy.interpolate as si
# 基于某种插值算法通过一组散点坐标，求得插值器函数
func = si.interp1d(
	离散点x坐标, 离散点y坐标,
    kind='使用的插值算法' # linear cubic
)
```

案例：

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo09_interpolate.py  插值器
"""
import numpy as np
import scipy.interpolate as si
import matplotlib.pyplot as mp

# 构建原始的散点数据
min_x = -50
max_x = 50
dis_x = np.linspace(min_x, max_x, 15)
dis_y = np.sinc(dis_x)
mp.scatter(dis_x, dis_y, c='red', s=60)
# 以这些散点为参数，获取插值器函数
linear = si.interp1d(dis_x, dis_y, kind='linear')
cubic = si.interp1d(dis_x, dis_y, kind='cubic')

# 把两个函数曲线画出来
x = np.linspace(min_x, max_x, 1000)
y1 = linear(x)
y2 = cubic(x)
mp.plot(x, y1, c='dodgerblue', label='Linear')
mp.plot(x, y2, c='orangered', label='Cubic')
mp.legend()
mp.show()
```

#### 积分

直观的说，对于一个给定的正实值函数，在一个实数区间上的定积分可以理解为坐标平面上由曲线、直线、轴组成的曲边梯形的面积值（一个确定的实数值）。

案例：y=2x<sup>2</sup> + 3x + 4在[-5,5]区间的函数图像。

```python
# 调用scipy.integrate模块的quad方法计算积分：
import scipy.integrate as si
area = si.quad(f, a, b)[0]
print(area)
```

#### 图像相关

```python
import scipy.ndimage as sn
# 高斯模糊
image2 = sn.median_filter(image, 21)
# 角度旋转
image3 = sn.rotate(image, 45)
# 边缘识别
image4 = sn.prewitt(image)
```

案例：

```python

```

























