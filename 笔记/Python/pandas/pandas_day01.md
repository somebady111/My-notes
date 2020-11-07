# pandas_day01

#### pandas的数组类型

`Series:`

> ```python
> import pandas as pd
> # 创建series数组
> s1 = pd.Series([3,-5,7,4],index=list('ABCD')
> ```
>
> 

`DataFrame:`

> ```python
> # 数据
> data = [['Belglum','Brussels',11190846],
>         ['Indla','New Delhi',1303171035],
>         ['Brazil','Brasilia',207847528]]
> 
> # 结构
> df = pd.DataFrame(data=data,index=[1,2,3],columns=['Country','Capital','Populatition'])
> ```
>
> 

`时间类型：`

> ```python
> # 规定时间
> date = pd.date_range('20100101',periods=6)
> 
> data = [['Belglum','Brussels',11190846],
>         ['Indla','New Delhi',1303171035],
>         ['Brazil','Brasilia',207847528]]
> 
> date = pd.DataFrame(np.random.randn(6,4),index=date,columns=list('abcd'))
> ```

|  函数   |        返回值        |
| :-----: | :------------------: |
| values  |         元素         |
|  index  |         索引         |
| columns |         列名         |
| dtypes  |         类型         |
|  size   |       元素个数       |
|  ndim   |        维度数        |
|  shape  | 数据形状（行列数目） |

####查改增删

`查：`

> ```python
> print(s1[1])
> print(s1['john'])
> print(s1[['leo','kate']])
> print(s1[s1<90])
> print(s1.index)
> 
> # 单列和多列
> print(s1.leo)
> print(s1['leo'])
> 
> # 单行和多行，head，tail，从头开始获取多行数据
> print(s1[0:1])
> print(s1.head(1))
> print(s1.tail)
> 
> # ：对某几行进行访问，不写访问全部
> print(data.loc[1:3,['gender','age']])
> print(data.iloc[:,[1,2]])
> print(data.iloc[:,1:3])
> 
> # loc和iloc
> # loc方法是针对DataFrame索引名称的切片方法
> # iloc接收的必须是行索引和列索引的位置
> print(data[1:3])
> print(data.iloc[1:3])
> 
> print(data.iloc[[1,3],0:2])
> 
> print(data[-3:-1])
> print(data.iloc[-3:-1])
> ```

`改：`

> 