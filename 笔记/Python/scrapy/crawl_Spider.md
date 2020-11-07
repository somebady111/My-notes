# crawl_Spider

- 通用性爬虫
- 创建项目方法
  - 1.scrapy startproject 项目名
  - 2.创建一个crawlSpider：
    - 1.定值一个模板：模板查看方法，scrapy genspider -l
    - 2.位置（spiders）：scrapy genspider -t crawl 名称 域名
  - 3.settings设置
  - 4.定义Rule规则
    - 根据网页需求不同定义不同的查询规则（如分页规则，获取链接规则）
    - Rule参数
      -  
    - allow：定义从当前页提取的链接复合allow中的正则规则
    - link_extractor--restrict_xpaths（从爬取的页面提取那些链接）
    - callback：回调函数
- 使用流程