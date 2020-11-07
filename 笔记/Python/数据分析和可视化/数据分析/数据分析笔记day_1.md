# 	数据分析和可视化

- 老师：徐铭 xuming@te'du.cn 15201603213

- 什么是数据分析：

  - 数据分析是指是指适用的统计分析方法对手机来的数据进行分析，提取有用的信息并形成结论，最终对数据加以详细研究和概括总结的过程

- 适用python做数据分析的常用库

  - 1.numpy

    - 基础数值算法

    - 概述

      - Numerical python
      - 其他数据分析和机器学习库的底层库，使用c语言实现，开源免费

    - 基础

      - ndarray数组

        - ```python
          import numpy as np
          a = np.array([1,2,3,4,5])
          print(a,a*3,type(a))
          ```

        - 提高内存的使用空间和使用效率，减少了实际数据的访问复杂度，提高性能

      - ndarray对象的创建

        - ```python
          # 创建ndarray对象
          a = np.array([1,...])
          # 步长为2
          np.arange(0,10,2)
          # 输出十个0
          np.zeros(10,dtype='int32')
          # 输出十个1
          np.ones(10,dtype='bool')
          # 创建一个与a结构相同的数组，元素全为0
          np.zeros_like(a)
          ```

        - dtype 设置数据类型，默认为float

      - ndarray对象属性的操作

        - 数组的维度 ndarray.shape

        - 元素的类型 ndarray.dtype

        - 数组元素的个数 ndarray.size

        - ```python
          # ndarray属性操作
          x = np.arange(1,7)
          print(x,x.shape)
          # 变维
          x.shape = (2,3)
          print(x,x.shape)
          # 维度
          y = np.array([[1,2,3],[4,5,6]])
          print(y,y.shape)
          # 数据类型
          z = np.array([1,2,3,4])
          print(z,type(z),z.dtype)
          # 转换类型
          z.astype('float32')
          print(z,type(z),z.dtype)
          # ndarray元素的个数
          d = np.arange(1,10)
          d.shape = (3,3)
          print(d,d.dtype,d.shape)
          print('len:',len(d),'size:',d.size)
          # 数组元素的索引(下标)
          u = np.arange(1,19)
          u.shape = (3,2,3)
          print(u)
          print(u[0][0])
          print(u[2,1,1])
          # 三层循环遍历u数组
          for i in range(u.shape[0]):
              for j in range(u.shape[1]):
                  for k in range(u.shape[2]):
                      print(u[i,j,k],end=' ')
          ```

    - ndarray对象属性操作详解

      - numpy内部数据类型

        

        - | 类型名   | 类型标识符                       |
          | -------- | -------------------------------- |
          | 有符号型 | int8 int16 int32 int 64          |
          | 无符号型 | uint8 uint16 uint32 uint64       |
          | 布尔型   | bool                             |
          | 浮点型   | float16 float32 float64          |
          | 复数型   | complex64 complex128             |
          | 字符串型 | str每个字符用32位Unicode编码表示 |

      - 自定义复合类型

        - ```python
          # 自定义复合类型(旧版本需要定义数据格式，见视频)
          data = [('a',[1,2,3],11),('b',[3,4,5],22),('c',[6,7,8],33)]
          a = np.array(data)
          print(a,a.size)
          print(a[0][1][0])
          # 日期类型数组
          f = np.array(['2011','2111-01-01','2013-12-19'])
          print(f,f.dtype)
          # numpy.datetime数据类型
          f1 = f.astype('M8[D]')
          print(f1)
          f2 = f1.astype('int32')
          print(f1[2]-f1[1])
          ```

        - 类型字符码

          

          - | 类型           | 字符码          |
            | -------------- | --------------- |
            | bool           | ?               |
            | int8/16/32/64  | i1/i2/i4/i8     |
            | uint8/16/32/54 | u1/u2/u4/u8     |
            | float16/32/64  | f2/f4/f8        |
            | complex64/128  | c8/c16          |
            | str_           | U               |
            | datetime64     | M8[D/M/Y/h/m/s] |

    - ndarray数组对象的维度操作

      - 视图变维(数据共享)

        - reshape()
        - ravel()

      - 复制变维（数据独立）：

        - flatten（）
        - copy()

      - 就地变维：

        - a.shape  
        - a.resize()

      - ```python
        # 维度操作,ab使用同一份数据
        # 视图变维
        a = np.arange(1,9)
        print(a,a.shape)
        b = a.reshape(2,4)
        print(b,b.shape)
        c = b.ravel()
        print(c,c.shape)
        
        print('-'*100)
        
        #复制变维
        a = np.arange(1,9).reshape(2,4)
        b = a.flatten()
        c = a.copy()
        print(a,a.shape)
        print(b,b.shape)
        print(c,c.shape)
        
        # 就地变维
        b.resize(4,2)
        print(b)
        ```

    - ndarray数组的切片操作

      - ```python
        # 数组的切片操作,二维
        a = np.arange(1,10).reshape(3,3)
        print(a)
        print(a[0:2,:2])
        b = np.arange(1,28).reshape(3,3,3)
        print(b[0,:2,:2])
        ```

    - ndarray数组的掩码操作

      - ```python
        # 掩码操作
        a = np.arange(1,11)
        mask = a%2 == 0
        print(a[mask])
        b = np.arange(1,101)
        mask = (b%3 ==0) | (b%7 ==0)
        print(b[mask])
        # 掩码排序
        name = np.array(['a','c','d','b'])
        indices = np.array([0,3,1,2])
        print(name[indices])
        ```

    - 多维数组的组合和拆分

      - 垂直方向操作
        - 合并
          - c = np.vstack((a,b))
        - 拆分
          - sa,b = np.vsplit(c,2)
      - 水平方向操作
        - 合并
          - c = np.hstack((a,b))
        - 拆分
          - a,b = np.hsplit(c,2)
      - 深度方向操作
        - 合并
          
          - c = np.dstack((a,b))
          
        - 拆分
          
          - a,b = np.dsplit(c,2)
          
        - ```python
          #多维数组的组合和拆分
          a = np.arange(1,7).reshape(2,3)
          b = np.arange(7,13).reshape(2,3)
          #垂直方向组合拆分
          c = np.hstack((a,b))
          print(c)
          a,b = np.vsplit(c,2)
          print(a,b,sep='\n')
          #深度拆分组合
          c = np.dstack((a,b))
          print(c)
          a,b = np.dsplit(c,2)
          print(a,b,sep='\n')
          ```

    - 多维数组的组合与拆分的相关函数

      - np.concatenate((a,b),axis=0)
        - 沿着axis轴向，把a和b组合在一起
        - axis = 0 垂直方向
        - axis = 1 水平方向
        - axis = 2 深度方向（a和b为三维数组）
      - np.split(c,2,axis=0)
        - 沿axis轴向，对c进行拆分，拆成两份

    - 简单的以为数组组合方案

      - np.row_stack((a,b))
        - a,b都为一位数组
        - 把两个数组放在一块形成两行
      - np.column_stack((a,b))
        - 把两个数组并在一起形成两列

    - ndarray的其他属性

      - shape
      
      - dtype
      
      - size
        
        - 元素个数
        
      - ndim
        
        - 维数（n维数组）
        
      - itemsize
        
        - 元素的字节数
        
      - nbytes
        
        - 总子节数
        
      - real
        
        - 复数数组实部
        
      - imag
        
        - 复数数组虚部
        
      - T
        
        - 转置数组
        
      - flat
        
        - 高维数组的扁平迭代器
        
      - ```python
        #其他属性
        a = np.array([[1+1j,2+1j,3+1j],
                      [4+1j,5+1j,6+1j],
                      [7+1j,8+1j,9+1j],])
        print(a.size)
        print(a.shape)
        print(a.dtype)
        print(a.ndim)
        print(a.nbytes)
        print(a.real,a.imag,sep='\n')
        print(a.T)
        print([elem for elem in a.flat])
        print(a.tolist())
        
        print('-'*100)
        #可视化
        import matplotlib.pyplot as mp
        a = np.array([91,92,20,44,66])
        mp.plot(a)
        mp.show()
        ```

  - 2.scipy

    - 科学计算

  - 3.matplotlib

    - 数据可视化

  - 4.pandas

    - 序列的高级函数
