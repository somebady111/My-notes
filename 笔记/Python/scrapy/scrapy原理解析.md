**1.组成和调度**

a.组成

1.Engine 引擎：数据流处理，触发事务，在整个编写过程中是无需编辑的

2.Item 项目：定义爬取结果的数据结构，爬取目标的需求字段

3.Scheduler 调度器：接收引擎Engine发送来的请求加入队列中，提供给引擎

4.Downloader 下载器：下载Item定义的内容，将网页内容返回给Spider

5.Spider 蜘蛛：定义爬取的规则（翻页解析、层级爬取）

6.Item Pipeline 项目管道：负责处理Spider从网页中抽取的项目（Item定义的字段），起到清洗、验证、存储的作用

7.Downloader Middlewares 下载器中间件：引擎Engine和Downloader之间的钩子（连接）框架，处理引擎到下载器之间的请求和响应

8.Spider Middlewares 蜘蛛中间件：引擎Engine和Spider之间的钩子（连接）框架，处理Spider到Engine之间的请求，响应和输出的结果及新的请求

b.调度

1.Engine先打开一个网站，找到处理该网站的spider，并向该spider请求第一个要爬取的URL

2.Engine从spider中获取到第一个要爬取的URL，并通过Scheduler以request的形式调度

3.Engine向Scheduler请求下一个爬取的URL

4.Scheduler返回下一个要爬取的URL给Engine，Engine将URL通过Downloader Middlewares发送给Engine

5.下载完毕后，Downloader生成该页面的response，通过Downloader Middlewares发送给Engine

6.Engine从下载器中接收到response，通过Spider Middlewares发送给spider处理

7.spider处理response，并返回提取到的Item及新的response给Scheduler

8.Engine将Spider返回的Item给Item Pipeline，将新的response给Scheduler

9.重复2-8的过程

**2.Downloader Middlewares用法：引擎到下载器之间的请求和响应**

 

