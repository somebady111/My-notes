# 19-11-24 周天



**1.复习知识点：JS事件和JavaScrap语法**

```html
// 作用1：写html输出
document.write("<h1>xxxxx</h1>");
// 作用2：对事件作出反应
<!DOCTYPE html>
<html>
<body>
    <button type="button" onclick="alert('Welcome!')">点击这里</button>
</body>
</html>
// 作用3：改变内容
<!DOCTYPE html>
<html>
<body>
    <p id="demo">
    	JavaScript 能改变 HTML 元素的内容。
    </p>
    <script>
        function myFunction()
        {
        x=document.getElementById("demo");  // 找到元素
        x.innerHTML="Hello JavaScript!";    // 改变内容
        }
    </script>
    <button type="button" onclick="myFunction()">点击这里</button>
</body>
</html>
// 雪碧图
<!DOCTYPE html>
<html>
<body>
    <script>
        function changeImage()
        {
        element=document.getElementById('myimage')
        if (element.src.match("bulbon"))
          {
          element.src="../i/eg_bulboff.gif";
          }
        else
          {
          element.src="../i/eg_bulbon.gif";
          }
        }
    </script>

    <img id="myimage" onclick="changeImage()" src="../i/eg_bulboff.gif">

    <p>点击灯泡来点亮或熄灭这盏灯</p>

</body>
</html>
// 改变css样式
<!DOCTYPE html>
<html>
<body>
    <p id="demo">
    	JavaScript 能改变 HTML 元素的样式。
    </p>
    <script>
        function myFunction()
        {
        x=document.getElementById("demo") // 找到元素
        x.style.color="#ff0000";          // 改变样式
        }
    </script>
    <button type="button" onclick="myFunction()">点击这里</button>
</body>
</html>
// 验证输入
<!DOCTYPE html>
<html>
<body>
    
    <p>请输入数字。如果输入值不是数字，浏览器会弹出提示框。</p>

    <input id="demo" type="text">

    <script>
        function myFunction()
        {
        var x=document.getElementById("demo").value;
        if(x==""||isNaN(x))
            {
            alert("Not Numeric");
            }
        }
    </script>

    <button type="button" onclick="myFunction()">点击这里</button>

</body>
</html>
// 访问某个html元素
document.getElementById(id)
// JS变量
var x = 2;
var y = 3;
var z = x + y;
// Javascript表单验证
<html>
    <head>
        <script type="text/javascript">
            function validate_required(field,alerttxt)
            {
            with (field)
              {
              if (value==null||value=="")
                {alert(alerttxt);return false}
              else {return true}
              }
            }

            function validate_form(thisform)
            {
            with (thisform)
              {
              if (validate_required(email,"Email must be filled out!")==false)
                {email.focus();return false}
              }
            }
        </script>
    </head>

    <body>
        <form action="submitpage.htm" onsubmit="return validate_form(this)" 	method="post">
            Email: <input type="text" name="email" size="30">
            <input type="submit" value="Submit"> 
        </form>
    </body>

</html>
```

**2.学习新内容：SQL语言**

1.select

```sql
# select从表中选取数据
select 列表名 from 表名;
```

2.distinct

```sql
# 查找表中不重复出现的值
select distinct 列表名 from 表名称;
```

3.where

```sql
# 条件查询
select 列表名 from 表名 where 列 运算符 值;
```

4.and & or

```sql
# 成立条件查询
select 列表名 from 表名 where (列='' or 列='') and 列=''
```

5.order by

```sql
# 查询排序
select 列1，列2 from 表 order by 列 desc(asc)；
```

6.insert

```sql
# 插入语句
insert into 表(列1，列2,...) values(值1，值2，...)；
```

7.update

```sql
# 更新内容
update 表名称 set 列名称=新值 where 列名称=某值；
```

8.delete

```sql
delete 列=值 from 表;
```

9.SQL Top

```sql
# 返回记录的数目 SQL
select * from table_name;
select top 2 * from 表；
select top 50 percent * from 表； # 提取百分之50的内容
# mysql
select * from 表 limit n;
# Oracle
select * from 表 where rownum<=n;
```

10.SQL like

```sql
# 模糊查询
select * from 表 where 列 like 'str%'; # 查询该列存在以str开头的信息
select * from 表 where 列 like '%str'; # 查询该列存在以str结尾的信息
select * from 表 where 列 like '%str%'; # 查询该列包含str的信息
select * from 表 where 列 not like '%str%'; # 查询该列不包含str的信息
```

# 19-12-2 苏州 东台市

**1.复习pyquery并尝试建立代理池获取66ip的免费代理**

1.pyquery的调用和属性获取

```python
from pyquery import PyQuery as pq
doc = pq(page.text)
attrs = doc('.class p').text()
```

2.周三晚：无收获。解决问题：kali虚拟机连接网络问题

**2.解决虚拟机联网问题**

1.kali

解决：net连接方式，关闭静态网络连接方式

2.Ubuntu

解决：net连接方式，在无线局域网配置信息不存在的情况下，此设置共享主机IP地址

3.pywifi 模块

```python
# 模块1：获取本机的无线网卡
from pywifi import *

def main():
	wifi = PyWiFi()
	try:	
        # 获取第一个无线网卡
		ifaces = wifi.interfaces()[0]
		if ifaces in [const.IFACE_DISCONECTED,const.IFACE_INACTIVE]:
			print('this connect live')
		else:
			print('not live')
	except Exception as e:
		print('error:',e)

main()
```

```python
# 模块2：扫描附近的wifi
from pywifi import *

def main():
    wifi = PyWiFi()
    ifaces = wifi.interfaces()[0]
    ifaces.scan()
    results = ifaces.scan_results()
    for result in results:
        # 获取的无线网络名称
        print(result.ssid)
        
main()
```

```python
# 模块3：使用字典或者输入密码进行连接
# 获取网卡信息->扫描周围wifi信号->尝试连接，保存信息
# 获取网卡模块 扫描wifi模块 破解wifi模块

from pywifi import *
import sys
import time

def get_interfaces():
    wifi = PyWiFi()
    ifaces = wifi.interfaces()[0]
    # 输出无线网卡的名称
    print(ifaces.name())
    # 断开无线网卡连接
    ifaces.disconnect()
    time.sleep(3)
    

def scan_wifi():
    pass
    
def crack_wifi():
    pass
    
if __name__ == "__main__":
    get_interfaces()
```



4.暂定的爬虫和开发项目

content：1.myworld  2.

**3.问题：Django的环境触发问题**

1.问题描述：python27版本停止维护后，往期应用的版本问题出现异常。

2.解决方案：在pycharm中对环境进行重新设定，删除Windows中的python27版本

