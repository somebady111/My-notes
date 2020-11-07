# 套接字名和DNS

**1.网络地址解析**

​	1.顶级域名（TLD），由各机构进行发放，注册一个域名时，就会增加一个域名条例，当用户需要解析请求域名，顶级域名服务器会把客户端的请求转至自己的域名服务器

​	2.主要的套接字方法

```python
import socket

mysocket = scoket.socket(...)
# TCP流的监听套接字调用
mysocket.accept()
# 将特定的本地地址分配给套接字
mysocket.bind(address)
# 通过套接字发送的数据传输至远程地址
mysocket.connect()
# 返回与套接字连接的远程地址
mysocket.getpeername()
# 返回套接字自身的本地端点地址
mysocket.getsockname()
# 返回udp套接字接受的内容
mysocket.recvfrom(sc,sockname)
# 向远程地址发送数据
mysocket.sendto(data,address)
```

​	3.现代地址解析，python中使用的方法：getaddrinfo（），该方法返回套接字的五个坐标，是将用户指定的主机名和端口号转换为可供套接字方便使用的地址的唯一函数。

```python
from pprint import pprint
infolist = socket.getaddrinfo('baidu.com','www')
pprint(infolist)

info = infolist[0]
s = socket.socket(*info[0:3])
pprint(info[4])
s.connect(info[4])
```

