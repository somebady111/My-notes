# 数据分析笔记（个人版）day_6

#### 数据读取和处理：

`数据读取文件的处理：`

> ```python
> # np.loadtxt
> import numpy as np
> def dm2ymd(s):
>     dmy = str(s,encoding='utf-8')
>     time = dt.datetime.strptime(dmy,'%d-%m-%Y')
>     ymd = time.date().strftime('%Y-%m-%d')
>     return ymd
> 
> dates,closing_prices,volumns = np.loadtxt(
>     '../da_data/aapl.csv',unpack=True,delimiter=',',usecols=(1,6,7),dtype='M8[D],f8,f8',converters=
>     {1:dm2ymd}
>     )
> ```

#### 刻度设置：

`主刻度设置：`

> ```python
> # 设置刻度定位器
> ax = mp.gca()
> # 设置主刻度
> ax.xaxis.set_major_locator(md.WeekdayLocator(byweekday=md.MO))
> ax.xaxis.set_major_formatter(md.DateFormatter('%d %b %Y'))
> # 设置次刻度定位器为日定位器
> ax.xaxis.set_minor_locator(md.DayLocator())
> # 转换时间类型
> dates = dates.astype(md.datetime.datetime)
> ```

#### obv线绘制：

```python
# 绘制OBV
diff_prices = np.diff(closing_prices)
sign_prices = np.sign(diff_prices)
obvs = volumns[1:] * sign_prices
```

#### 根据条件设置不同颜色：

```python
# 柱子朝上,设置不同颜色
c = np.zeros_like(sign_prices).astype('str_')
c[sign_prices >= 0] = 'red'
c[sign_prices == -1] = 'green'
```

#### 矩阵：

`矩阵操作：`

> ```python
> a = np.arange(1,10).reshape(3,3)
> m = np.matrix(a,copy=True)
> m.T
> n.T
> ```



