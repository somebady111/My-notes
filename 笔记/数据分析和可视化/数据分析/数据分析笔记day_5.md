# 数据分析笔记（个人版）day_5

####布林带：

`加权卷积：`

> ```python
> # 加权卷积的5日均线
> kernel = np.exp(np.linspace(-1, 0, 5))[::-1]
> kernel /= kernel.sum()
> sma53 = np.convolve(closing_prices, kernel, 'valid')
> mp.plot(dates[4:], sma53, c='red',
>         label='SMA-53')
> ```

`填充颜色：`

> ```python
> # 填充颜色
> mp.fill_between(
>     dates[4:], uppers, lowers, uppers > lowers,
>     color='dodgerblue', alpha=0.3)
> 
> # 时间文本格式化
> mp.gcf().autofmt_xdate()
> ```

`预测点和预测线：`

> 