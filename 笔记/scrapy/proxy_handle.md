# proxy

**1.urllib爬虫代理：**

`1.urllib代理方法`

```text
代理内容：是http还是https，proxy = 'ip:port' proxy_handler = ProxyHandler()
设置代理：build_opener(proxy_handler)
访问链接：opener.open()
```

`2.认证代理：`

```python
proxy = 'user:password@ip:port'
```

`3.socks5代理`

```python
import socks
socks.set_default_proxy(socks.SOCKS5,'ip',port)
```

**2.requests爬虫代理**

```python
response = requeests.get(url,proxies=proxies)
```

**3.代理池设计思路**

```text
模块划分：存储模块、获取模块、检测模块、接口模块
存储模块设置分值
```

