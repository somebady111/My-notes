# 数据可视化学习笔记（个人版）day_7

#### day07

**奇异值分解：**

`拆解：`

> U,sv,V = np.linalg.svd(image,full_matrices=False)
>
> full_matrices 填充矩阵，当矩阵部位方形矩阵时，进行填充运算

**读取文件：**

`读值：`

> `图片：`
>
> import scipy.misc as sm
>
> image = sm.imread('文件地址'，True)  True进行灰度处理
>
> `文件：`
>
> np.loadtxt('文件名',unpack=True,delimiter=',',usecols=(读取行))

**画图：**

`同图绘制：`

> import matplitlib.pyplot as mp
>
> mp.figure('Image name')  name 窗口小图标名称
>
> mp.title('name',fontsize=14)  标题和文字大小
>
> mp.subplot(221) 221 两行两列第一个
>
> mp.xticks([])  去除x轴的刻度
>
> mp.yticks([])  去除y轴刻度
>
> mo.imshow(image,cmap='gray')  展示图片
>
> mp.tight_layout()  调整窗口布局
>
> mp.show()  图片显示

#### 傅里叶拆解：

`傅里叶：`

> ```python
> import numpy.fft as nf
> # 拆解
> complex_arry = nf.fft(y)
> # 反拆解
> y2 = nf.ifft(complex_arry)
> ```

#### 整理数据：

`数据处理：`

> ```python
> x = np.linspace(-2*np..pi,2*np.pi,10000)
> n = 1000
> y = np.zeros(n)
> for i in range(1,n+1):
> 	y += 4/(2*i-1)*np.pi*np.sin((2i-1)x)
> ```

#### 掩码操作：

> ```python
> # 掩码[]
> mp.plot(freqs[freqs>0])
> ```

#### 单图绘制：

> ```python
> # 绘制轴和标签，alpha透明度
> mp.plot(x,y1,label='y1',alpha=0.3)
> ```

#### 音频生成：

> ```python
> import scipy.io.wavfile as wf
> wf.write('文件地址'，sample_rates,filter_sigs.real,astype(np.int16))s
> ```

#### 二项分布：

> ```python
> # 10次，概率0.3，取100000点
> a = np.random.binomial(10,0.3,100000)
> ```

#### 超几何分布：

> ```python
> b = np.random.hypergeometric(25,5,5,10000)
> ```

#### 排序：

`联合简介排序：`

> ```python
> product = np.array(['Apple','Huawei','Mi','Oppo','Vivo'])
> prices = np.array([8888,4999,2999,3999,5999])
> vols = np.array([100,80,40,90,70])
> 
> indices = np.lexsort((vols,prices))
> print(product[indices])
> ```

`插入排序：`

> ```python
> d = np.array([1,2,3,5,7,9])
> e = np.array([4,6,8])
> indices = np.searchsorted(d,e)
> print(indices)
> f = np.insert(d,indices,e)
> print(f)
> ```

`插值器：`

> ```python
> min_x = -50
> max_x = 50
> dis_x = np.linspace(min_x,max_x,13)
> dis_y = np.sinc(dis_x)
> 
> mp.scatter(dis_x,dis_y,c='red',s=60)
> ```

#### 求积分的方法：

`手动：`

> ```python
> def f(x):
>     return 2*x**2+3*x+4
> 
> a,b = -5,5
> x = np.linspace(a,b,1000)
> y = f(x)
> 
> n = 50
> x2 = np.linspace(a,b,n+1)
> y2 = f(x2)
> area = 0
> for i in range(n):
>     area += (y2[i]+y2[i+1])*(x2[i+1]-x2[i])/2
> 
> print(area)
> ```

`numpy方法：`

> ```python
> import scipy.integrate as sn
> area1 = sn.quad(f,a,b)[0]
> print(area)
> ```

#### 图片处理：

> 
>
> ```python
> import scipy.ndimage as sn
> # 高斯模糊
> image2 = sn.median_filter(image,10)
> # 角度旋转
> image3 = sn.rotate(image,45)
> # 边缘检测
> image4 = sn.prewitt(image)
> ```

