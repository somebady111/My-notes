# pyspider

**1.pyspider在Linux安装时出现错误的解决方案：**

```python
# 下载依赖模块,这里要注意权限问题加sudo
sudo apt-get install libcurl4-gnutls-dev
```

**2.pyspider在Linux成功安装后出现插件版本不匹配：**

1.原因是因为WsgiDAV发布了版本 pre-release 3.x。

2.解决方法如下：

```python
# 在安装包中找到pyspider的资源包，然后找到webui文件里面的webdav.py文件打开，修改第209行即可。

# 把
'domaincontroller': NeedAuthController(app),
# 修改为：

'http_authenticator':{
        'HTTPAuthenticator':NeedAuthController(app),
    },

# 然后再执行pyspider all就能够通过http://localhost:5000打开页面了。
```

3.如何找到pyspider

```python
 # 具体看错误提示
    File "c:\python36\lib\site-packages\pyspider\webui\app.py", line 59, in run
    from .webdav import dav_app
```

**3.内置方法：**

1.crawl_config属性，项目的所有爬取配置：请求头，代理。

2.on_start()爬取入口，参数crawl()方法新建一个爬取请求，callback返回给指定的解析方法

3.index_page()接收response参数，doc（）方法传入相应的css选择器

4.detail_page()接收response参数，详情页信息，保存或打印

**4.pyspider用法详解：**

1.启动：

```python 
pyspider all
# 参数配置
pyspider [OPTIONS] COMMAND [ARGS]
# 参数[OPTIONS]
-c 指定配置文件名称
--logging-config  TEXT日志配置文件名称，默认为pyspider/logging.conf
--debug 开启调试模式
--queue-maxsize  INTEGER队列的最大长度
--message-queue TEXT消息队列连接字符串，默认multiprocessing.Queue
--phantomjs-proxy TEXT,PhantomJS使用代理ip:port形式
--data-path TEXT数据库存放路径
--version pyspider版本
--help 帮助信息
```

2.crawl（）方法

```python
url
callback # 回调函数
index_page() # 页面解析函数
age # 任务有效时间
priority # 爬取任务优先级
exetime # 设置定时任务
retries # 定义重试次数，默认为3
itag # 设置判定网页是否发生变化的节点值
auto_recrawl # 自动开启
method # HTTP请求方式，默认是GET
params # 定义GET请求参数
data # POST表单数据，请求为POST时，通过此参数进行传递表单数据
files # 上传文件，指定文件名
user_agent # 设置请求User_Agent
headers # 设置请求headers
cookies # 设置爬取使用的cookies
connect_timeout # 设置最长等待时间
timeout # 抓去网页的最长等待时间
allow_redirects # 确定是否自动处理重定向，默认为True
validate_cert # 确定是否验证证书，对HTTPS有效，默认为True
proxy # 爬取使用的代理
fetch_type # 开启对PhantomJS渲染
save # 在不同方法之间传递参数
cancel # 取消任务
```

**5.index_page()解析方法;**

```python

```

