# 		scrapy_framework

- 1.scrapy框架的简单使用（异步加载下载功能，了解scrapy框架的机制）

  - settings中的配置及参数详解

    - 修改一（不遵从robots协议）：ROBOTSTXT_OBEY = False
    - 修改二（调用请求头）：DEFAULT_REQUEST_HEADERS = {}
    - 修改三（打开管道）：ITEM_PIPELINES = {'Lianjia.pipelines.LianjiaPipeline' : 100,}
    - 修改四（设置日志级别）：
      - 位置：一般放在robots协议设置下
      - LOG_LEVEL = " "
        - CRITICAL ：严重错误
        - ERROR    ：普通错误
        - WARNING  ：警告信息
        - INFO     ：一般信息
        - DEBUG    ：调试信息
      - LOG_FILE = '文件名.log'

  - 创建项目流程

    - 1.创建一个新项目：scrapy startproject 项目名称
    - 2.新建一个spider：
      - 1.cd spiders
        - scrapy genspider 名称 域名
    - 3.修改开始start_url:开始请求的第一个URL
    - 4.终端测试运行：scrapy crawl httpbin（spiders文件夹下）
    - 5.更改settings设置
    - 6.程序启动运行
      - cd到最外层文件夹下
      - 新建begin.py文件
        - from scrapy import cmdline
          cmdline.execute('scrapy crawl 名称'.split()) # spider下的文件名称

  - 框架请求流程

    - Engine开始通栏全局,向Spider索要URL

    - Engine拿到URL后,交给Scheduler入队列

    - Scheduler将请求出队列,通过Downloader Middlewares交给Downloader下载

    - Downloader下载完成,把response交给Spider

    - Spider解析完成后，把数据交给Item Pipeline处理

      **把新的需要跟进的URL交给Engine,继续循环**

  - 4-16小总结

    - 1.创建项目
      - 1.创建总项目
      - 2.创建spiders
    - 2.设置settings
    - 3.更改spiders中的相关属性
    - 4.构造请求的URL
    - 5.从返回的response中提取信息
    - 6.items中的要爬取的属性
    - 7.pipelines管道中存入当前文件或数据库
    - 8.settings设置相关信息
      - 1.数据库的相关配置
      - 2.下载到本地文件的相关配置

- 2.scrapy和selenium的对接

  - 实战项目：淘宝商城爬取
  - 

- 3.scrapy通用爬虫

- 4.scrapy对接splash