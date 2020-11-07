# 数据分析DAY01

徐铭  xuming@tedu.cn  15201603213

[TOC]

**什么是数据分析？**

数据分析是指用适当的统计分析方法对收集来的数据进行分析，提取有用的信息并形成结论，最终对数据加以详细研究和概括总结的过程。

**使用python做数据分析的常用库**

1. numpy           基础数值算法
2. scipy               科学计算
3. matplotlib     数据可视化
4. pandas          序列的高级函数

## numpy概述

1. Numerical Python, 补充了python语言所欠缺的数值计算能力。
2. Numpy是其他数据分析及机器学习库的底层库。
3. Numpy完全标准C语言实现，效率充分优化。
4. Numpy开源免费。

## numpy的历史

1. 1995年， Numeric库。   Python数值运算的扩充。
2. 2001年，Scipy-> Numarray， 多维数组运算。

3. 2005年， Numeric+Numarray -> Numpy
4. 2006年， Numpy脱离Scipy成为独立的项目。



## numpy基础

### ndarray数组

```python
import numpy as np
# 创建ndarray数组
a = np.array([1, 2, 3, 4, 5])
print(a, type(a))
a = a * 3
print(a, type(a))
```

#### 内存中的ndarray对象

**元数据(metadata)**

存储对目标数组的描述信息， 如：dim count， dtype等。

**实际数据**

存储完整的数组数据。

将实际数据与元数据分开存放， 一方面提高了内存空间的使用效率， 另一方面减少了对实际数据的访问复杂度，提高性能。

#### ndarray对象的创建

```python
a = np.array([1, ....])
# 构建从0开始， 到10结束， 步长为2的数组
np.arange(0, 10, 2)
# 创建10个元素全为0的数组  
# dtype用于设置元素的数据类型, 默认为float
np.zeros(10, dtype='bool')
# 创建10个元素全为1的数组
np.ones(10)
```

案例：

```python
import numpy as np

a = np.array([1, 2, 3, 2, 1])
print(a)
b = np.arange(1, 10, 1)
print(b)

c = np.zeros(10, dtype='int32')
print(c)
d = np.ones(10, dtype='bool')
print(d)
# 创建一个与a的结构相同的数组，元素全为0
e = np.zeros_like(a)
print(e)
```

#### ndarray对象属性的基本操作

数组的维度：ndarray.shape

```python
a = np.arange(1, 7)
print(a, a.shape)
# 变维
a.shape = (2, 3)
print(a, a.shape)
# 创建2维数组
b = np.array([[1, 2, 3], [4, 5, 6]])
print(b, b.shape)
```

元素的类型：ndarray.dtype

```python
# 元素的数据类型
c = np.array([1, 2, 3, 4, 5, 6])
print(c, type(c), c.dtype)
# 转换c对象元素的dtype
c = c.astype('float32')
print(c, type(c), c.dtype)
```

数组元素的个数：ndarray.size

```python
# 元素的个数
d = np.arange(1, 10)
d.shape = (3, 3)
print(d, d.dtype, d.shape)
print('len:', len(d), '  size:', d.size)
```

数组元素的索引(下标)

```python
# 元素的索引操作
e = np.arange(1, 19)
e.shape = (3, 2, 3)
print(e)
print(e[2])
print(e[2][0])
print(e[2][0][0])
print(e[2, 0, 0])
# 尝试使用3层for循环嵌套遍历e数组
for i in range(e.shape[0]):
    for j in range(e.shape[1]):
        for k in range(e.shape[2]):
            print(e[i, j, k], end=' ')
```

#### ndarray对象属性操作详解

**numpy内部基本数据类型**

| 类型名     | 类型表示符                         |
| ---------- | ---------------------------------- |
| 布尔型     | bool_                              |
| 有符号整型 | int8  int16  int32  int64          |
| 无符号整型 | uint8  uint16  uint32  uint64      |
| 浮点型     | float16  float32  float64          |
| 复数型     | complex64  complex128              |
| 字串型     | str_ 每个字符用32位Unicode编码表示 |

**自定义复合类型**

```python
"""
demo04_cls.py  测试自定义复合类型
"""
import numpy as np

data = [('zs', [95, 97, 98], 15),
        ('ls', [85, 87, 88], 16),
        ('ww', [75, 77, 78], 17)]

# 使用dtype定义每个元素的数据类型
# 2个Uncode字符， 3个int32整数， 1个int32
a = np.array(data, dtype='U2, 3int32, int32')
print(a)
print('ls scores:', a[1]['f1'])

# 第二种设置dtype的方式
b = np.array(data,
             dtype=[('name', 'str_', 2),
                    ('scores', 'int32', 3),
                    ('age', 'int32', 1)])
print(b)
print(b[2]['name'], b[2]['age'])

# 第三种设置dtype的方式
c = np.array(
    data, dtype={
        'names': ['name', 'scores', 'age'],
        'formats': ['U2', '3int32', 'int32']})
print(c)
print(c[0]['name'])

# 第四种设置dtype的方式
d = np.array(
    data, dtype={'name': ('U3', 0),
                 'scores': ('3i4', 16),
                 'age': ('int32', 28)})
print(d)

# 测试日期类型数组
f = np.array(['2011', '2012-01-01',
              '2013-01-01 01:01:01',
              '2011-01-01'])
print(f, f.dtype)
# 把f中的元素当做numpy.datetime数据类型，
# 精确到Day
g = f.astype('M8[D]')
print(g, g.dtype)
h = g.astype('int32')
print(h, h.dtype)
print(g[1] - g[0])
```

**类型字符码**

| 类型           | 字符码          |
| -------------- | --------------- |
| bool_          | ?               |
| int8/16/32/64  | i1/i2/i4/i8     |
| uint8/16/32/64 | u1/u2/u4/u8     |
| float16/32/64  | f2/f4/f8        |
| complex64/128  | c8/c16          |
| str_           | U               |
| datetime64     | M8[D/M/Y/h/m/s] |

```
3i4   U8  u8   M8[h]
```

**ndarray数组对象的维度操作**

**视图变维(数据共享)**： reshape()  ravel()

```python
import numpy as np

a = np.arange(1, 9)
print(a, a.shape)
b = a.reshape(2, 4)
print(a, a.shape)
print(b, b.shape)
# a与b使用的是同一份数据
a[0] = 999
print(a, a.shape)
print(b, b.shape)

c = b.ravel()
print(c, c.shape)
```

**复制变维(数据独立)**： flatten()  copy()

```python
print('-' * 45)
a = np.arange(1, 9).reshape(2, 4)
b = a.copy()
print(a, a.shape)
print(b, b.shape)
a[0, 0] = 999
print(a, a.shape)
print(b, b.shape)
```

**就地变维**： a.shape   a.resize()

```python
print('-' * 45)
print(b)
b.resize(4, 2)
print(b)
```

#### ndarray数组的切片操作

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo06_sp.py 切片操作
"""
import numpy as np

# a[起始位置:终止位置:步长]
a = np.arange(1, 10)
print(a)
print(a[:3])
print(a[3:6])
print(a[6:])
print(a[::-1])
print(a[:-4:-1])
print(a[-4:-7:-1])
print(a[::])
print(a[::3])
print(a[1::3])
print(a[2::3])

print('-' * 45)
a = a.reshape(3, 3)
print(a)
print(a[:2, :2])

a = np.arange(1, 28).reshape(3, 3, 3)
# 0页中的前两行前两列
print(a[0, :2, :2])
```

#### ndarray数组的掩码操作

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo07_mask.py   ndarray的掩码操作
"""
import numpy as np

a = np.arange(1, 11)
print(a)
mask = a % 2 == 0
print(mask)
print(a[mask])

# 输出100以内 3与7的倍数
a = np.arange(1, 100)
mask = (a % 3 == 0) | (a % 7 == 0)
print(a[mask])

# 使用掩码完成原数组的排序
names = np.array(
    ['Tom', 'Jerry', 'Lily', 'Lucy'])
indices = np.array([3, 2, 0, 1])
print(names[indices])
```

#### 多维数组的组合与拆分

垂直方向的操作：

```python
# 垂直方向合并
c = np.vstack((a, b))
# 垂直方向拆分
a, b = np.vsplit(c, 2)
```

水平方向的操作：

```python
# 水平方向合并
c = np.hstack((a, b))
# 水平方向拆分
a, b = np.hsplit(c, 2)
```

深度方向的操作：

```python
# 深度方向合并
c = np.dstack((a, b))
# 深度方向拆分
a, b = np.dsplit(c, 2)
```

案例：

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
"""
demo08_stack.py   多维数组的组合与拆分
"""
import numpy as np

a = np.arange(1, 7).reshape(2, 3)
b = np.arange(7, 13).reshape(2, 3)
# 水平方向的组合与拆分
c = np.hstack((a, b))
print(c)
a, b = np.hsplit(c, 2)
print(a, b, sep='\n')

# 垂直方向的组合与拆分
c = np.vstack((a, b))
print(c)
a, b = np.vsplit(c, 2)
print(a, b, sep='\n')

# 深度方向的组合与拆分
c = np.dstack((a, b))
print(c)
a, b = np.dsplit(c, 2)
print(a, b, sep='\n')
print(a.ravel())
print(b.ravel())
```

多维数组的组合与拆分相关函数

```python
# 沿着axis轴向，把a与b组合在一起
#   axis=0：垂直方向
#   axis=1：水平方向
#   axis=2：深度方向 （要求a与b为三维数组）
np.concatenate((a, b), axis=0)
# 沿着axis轴向，对c进行拆分  拆成2份
np.split(c, 2, axis=0)
```

简单的一维数组的组合方案：

```python
# a， b都为1维数组
# 把两个数组摞在一起形成两行
np.row_stack((a, b))
# 把两个数组并在一起形成两列
np.column_stack((a, b))
```

#### ndarray类的其他属性

* size        元素的个数
* ndim      维数（n维数组的那个n）
* itemsize    元素的字节数
* nbytes    总字节数
* real         复数数组的实部
* imag       复数数组的虚部
* T             转置视图
* flat         高维数组的扁平迭代器