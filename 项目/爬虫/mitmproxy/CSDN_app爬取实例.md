# CSDN_APP爬取

**1.获取节点**

1.标题：Title

2.内容：TextBody

3.作者：NickName

4.时间：UpdateTime

5.文章链接：ArticleUrl

9.阅读人数:ViewCount

**2.修正**

1.pymongo报错：脚本运行正常，但是使用mitmproxy -s xx.py 会报错。猜想：1.运行环境问题2.运行命令不支持，或运行在2.0版本；这个问题也许可以在其他系统上可以解决

2.数据保存：只要数值型数据，进行数据分析

3.文章标题 阅读人数 评论人数 点赞数（csv格式保存）

4.推荐文章：

文章列表页面：https://gw.csdn.net/cms-app/v1/home_page/may_login/list_recomment_articles?shown_offset=0&category=home&size=20%cookieid=aimei867291036876537&type=more

|   需求   |    标签    |
| :------: | :--------: |
| 文章链接 |    url     |
|   日期   | created_at |
|   标题   |   title    |
| 内容梗概 |  summary   |
|   作者   |  nickname  |
| 查看人数 |   views    |
| 评论人数 |  comments  |

5.失误点

获取返回的json串时，没用获取到目的json串，正确的为data下的articles

6.阶段

文章列表的内容已经成功获取，下一阶段，获取文章详情的相关信息和在mongodb数据库上的测试

7.问题

mongodb数据库不能远程连接，证书的问题

mongodb数据库不能在19.6Ubuntu版本使用，版本不支持，只支持16.04的LTS

8.文章详情

https://gw.csdn.net/cms-app/v1/blog_details/may_login/get_article_details_info_html?from=home&articleId=98121556&bloggerUserName=qq_43431158

9.文章详情需求

|   需求   |     标签     |
| :------: | :----------: |
|   标题   |    Title     |
|   内容   |   TextBody   |
| 发表时间 |   PostTime   |
| 评论数量 | CommentCount |
| 点赞数量 |     Bury     |
|   作者   |   UserName   |
| 查看数量 |  ViewCount   |
|   类别   | ChannelName  |
|          |              |

问题未解决！

10.简单的数据分析

对比1：标题title  阅读量：views

对比2：标题title  评论量：comments

分析对于文章阅读数和评论数，拟定文章的关注度

工具：三维坐标图
