# python3.8新功能

**1.赋值表达式：**

​	赋值表达式变量可以不存在，无序再创建一个单独语句。如果在3.8以下的版本，不去定义一个line变量，则报出line没有被定义。

``` python
while (line := file.readline())!='end':
    print(chunk)
```

**2.仅通过位置指定参数：**

​	通过/分隔位置参数和关键字参数。

```python	
def pow(x,y,z=3,/):
    r = x**y
    if z is not 3:
        r %= z
    return r
```

**3.f字符串调试：**

​	在同一个表达式内进行输出文本和值、变量的计算。

```python
x = 3
print(f'{x+1}')
```

**4.多进程共享内存：**

​	在multiprocessing模块中提供了SharedMemory类，可以在不同的python进程之间创建共享的内存区域。

**5.Typing模块：**

​	typing模块，添加类型提示，供第三方工具验证python代码。

​	final修饰器和Final类型标注表名，被修饰对象或被标注对象在任何时候都不应该被重写、继承、重新赋值

​	Literal类型将表达式限定为特定的值或值得列表

​	TypedDict可以创建字典，其特定键的值被限制在一个或多个类型上。这些限定仅用于编译时确定值的合法性，而不能在运行时进行限制。

**6.新版本的pickle协议：**

​	pickle提供了一种序列化和反序列化python数据结构或实例的方法，可以将字典原样保存下来以供读取。新版本支持范围更广、更强大、更有效的序列化。python3.8支持的pickle可以用一种新方法pickle对象，它支持python的缓冲区协议，如bytes，memoryviews或numpy array，新的pickle避免了在pickle这些对象时的内存复制操作。

**7.可翻转字典：**

​	python3.6的字典还会继承元素的顺序，而python3.8的字典允许使用reverseed（）方法

**8.性能改进：**

​	改进了内置方法和函数的速度

​	一个新的opcode缓存可以提高解释器中指定指令的速度目前是（LOAD_GLOBAL opcode）

​	文件复制操作：shutil.copyfile()和shutil.copytree()现在使用平台特定的调用和其他优化措施，提高操作速度

​	新建列表变小

​	python3.8面向新型类（class A(object)）的类变量中写入操作更快。operator.itemgetter()和collections.nameduple()得到优化

**9.python C API 和 Cpython实现**

**10.新的python3.8下载地址**



​	