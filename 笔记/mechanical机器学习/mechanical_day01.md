# mechanical_day01

`经典机器学习框架：scikit-learn(sklearn)`

> ```python
> # 均值移除（数据预处理）
> import sklearn.preprocessing as sp
> sp.scale(原始样本矩阵)# 返回值：均值移除后的样本矩阵
> ```

> ```python
> # 范围缩放(线性缩放)
> import sklearn.preprocessing as sp
> mms = sp.MinMaxScaler(feature_range=(最大值，最小值))
> mms.fit_transform(原始样本矩阵) # 返回值：范围缩放后的样本矩阵
> ```
>
> ```python
> # 归一化（移除并范围缩放,l1范数）
> import sklearn.preprocessing as sp
> sp.normalize(原始样本矩阵，norm='l1')# 返回值：归一化后的样本矩阵
> ```
>
> ```python
> # 二值化
> bin = sp.Binarizer(threshold=阈值)
> bin.transform(原始样本矩阵) # 二值化后的样本矩阵
> ```
>
> ```python
> # 独热编码
> ohe = sp.OneHotEncoder(sparse=是否采用稀疏矩阵的方式，默认为True，dtype=元素类型)
> ohe.fit_trnsfrom(原始样本矩阵) # 返回值：独热编码后的样本矩阵
> ```
>
> ```python
> # 标签编码（文本形式的特征值编码转换为数字）
> lbe = sp.LabelEncoder()
> lbe.fit_transform(原始特征向量) # 编码后的特征向量
> lbe.inverse_transform(编码后的特征向量)# 原始样本矩阵
> ```
>
> ```python
> # 梯度下降算法（计算最小误差，和最优解）
> # 见代码
> ```
>
> ```python
> # 线性回归
> import sklearn.linear_model as lm
> model = lm.LinearRegression()
> model.fit(输入矩阵，输出向量)
> model.predict(输入矩阵) # 返回值：输出向量
> ```
>
> ```python
> # 岭回归
> import sklearn.linear_model as lm
> model = lm.Ridge(正则强度，fit_intercept=调整截距，max_iter=最大迭代次数)
> ```
>
> ```python
> # 多项式回归
> import sklearn.preprocessing as sp
> import sklearn.linear_model as lm
> sp.PolynomialFeatures(n) # 多项式特征扩展器
> lm.LinearRegression() # 线性回归器
> ```
>
> ```python
> # 决策树
> ```
>
> 

`深度（神经网络）学习框架：TensorFlow`

> 

`TensorFlow的高级封装：tflearn`

> 