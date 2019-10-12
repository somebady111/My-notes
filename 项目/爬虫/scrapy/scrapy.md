# 	新浪微博爬取

- 

# 知乎网爬取

- 

# 中华网科技

- 

# bing美图爬取（完成）

- 第一步分析网页获取异步加载的源地址
  - https://cn.bing.com/images/async?q=%e9%9e%a0%e5%a9%a7%e7%a5%8e&first=40&count=35&relp=36&cw=1129&ch=746&scenario=ImageBasicHover&datsrc=N_I&layout=ColumnBased&mmasync=1&IG=3EA0C21D86C74BA88C00C43273A6280E&SFX=2&iid=images.5610
  - https://cn.bing.com/images/async?q=%e9%9e%a0%e5%a9%a7%e7%a5%8e&first=78&count=35&relp=71&cw=1129&ch=746&scenario=ImageBasicHover&datsrc=N_I&layout=ColumnBased&mmasync=1&dgState=c*6_y*2681s2665s2601s2558s2587s2652_i*72_w*174&IG=3EA0C21D86C74BA88C00C43273A6280E&SFX=3&iid=images.5610
  - https://cn.bing.com/images/async?q=%e9%9e%a0%e5%a9%a7%e7%a5%8e&first=121&count=35&relp=106&cw=1129&ch=746&scenario=ImageBasicHover&datsrc=N_I&layout=ColumnBased&mmasync=1&dgState=c*6_y*3868s3814s3858s3814s3924s4038_i*107_w*174&IG=3EA0C21D86C74BA88C00C43273A6280E&SFX=4&iid=images.5610
- 难点
  - ajax模拟请求
  - scrapy框架
- 1.items
  - 设置爬取的属性
- 2.settings
  - robots协议
  - 管道...
- 3.spiders
  - 1.start_requests
    - 构造URL异步请求地址
    - 找出每页面URL的规律
    - 返回每个页面的URL给self.parse
  - 2.parse
    - 1.构造items对象
    - 2.xpath匹配每个地址中的URL
    - 3.返回图片的URL给item
- 4.pipelines
  - 定义imagepipeline
  - 图片和数据库保存

# 智联招聘爬取（未完成）

- 使用工具
  - scrapy+selenium+phantomJS+mongoDB
- 网站分析
  - URL：<https://www.zhaopin.com/>
  - 使用requests返回的是注册页面的源码
  - 策略：使用selenium+scrapy爬取
  - 选择输入框的id，输入搜索的关键词
  - 模拟点击+解析页面
- 开始项目
  - 创建项目zhaopin
- 分解页面
  - 一级页面 
    - 通过关键词传输构造请求URL
    - 最大页数构造URL的iteration
  - 二级页面
    - 解析

# 淘宝爬取（未完成）

- 使用工具
  - scrapy+selenium+phantomJS+mongoDB
- 网站分析
  - URL：<https://www.taobao.com/>
- 开始项目
  - 1.确定项目名称-建立项目
    - scrapyselenium
    - scrapy startproject scrapyselenium
  - 2.新建spider
    - 文件名+域名
    - scrapy genspider taobao www.taobao.com
- 网站分析
  - 实现功能
    - 选择输入框，输入搜索的关键词

# oppo应用中心游戏数据

- 

# 美团外卖网站

- 

# 爱美时尚网图片爬取（完成）

`技术点：`

> 1.scrapy框架

`具体分析：`

> 1.`网址：`<http://www.aimeishishang.com/meinvtupian/>
>
> #### 创建爬虫项目：
>
> `项目名称：`
>
> 1.aimei
>
> 2.scrapy startproject aimei
>
> `创建spider：`
>
> 1.scrapy genspider aimei www.aimeishishang.com
>
> `确定爬取的内容：`
>
> 1.获取一级页面的title作为文件名
>
> 2.获取二级页面的中的图片URL
>
> `管道下载：`
>
> 1.配置下载管道，下载图片

`问题：`

> `权限访问问题：`
>
> 1.游客身份出现权限问题，访问的图片不存在，但基本可以保证页面是访问成功的
>
> `管道重定义问题：`
>
> 1.管道的设置是连接到框架的定义的程序的，class ImagePipeline和file_path中的URL是不改变的
>
> 2.get_media_requests负责处理遍历Item中的URL，并下载到指定的文件夹
>
> 3.item_completed负责清洗不存在的数据
>
> 4.file_path函数用于处理文件名称

`优化方案：`

> 1.选择类别
>
> 2.分类下载
>
> 3.保存在本地数据库

**上海赶集网<http://sh.ganji.com/fanggongyu/>：（未完成）**

`1.分析：`

>   ```text
>   对不同地区的房价进行分析，求出该地区的平均房价
>   搜索不用条件下：价格，地点，大小，标签
>   ```

`2.需求：`

>   `一级页面：`标题title、类型type、价钱price
>
>   `二级页面：房屋资料content，电话telphone

**拉勾网爬取：**

1.访问会出现子页面地点选择，需要切换主页面和子页面点击后切换主页面继续获取内容