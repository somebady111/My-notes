# 优化python语法

**1.循环遍历**

```python
# 遍历下标和元素的优化，函数enumerate()
# 联合遍历优化zip()函数
# product扁平化多层嵌套循环
from itertools import product
def find_page(page1,page2,page3):
    for i,p,k in product(page1,page2,page3):
        print(i,p,k,sep='\n')
```

**2.中断循环**

```python
# 使用情况：判断条件不符合的情况下，使用break跳出，这里使用takewhile()函数来简化
from itertools import takewhile
# 遍历参数，判断函数，遍历数组，列表或者元组
for user in takewhile(is_qualified,users):
    do_data()
```

**3.使用生成器来编写修饰函数**

```python
def certificate(data):
    ...
    yield n
    
def do_data(datas):
    for data in certificate(datas):
        return data
```

