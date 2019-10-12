# 	Django创建项目

**1.创建独立的虚拟环境**

>   ```python
>   python3.7 -m venv typeidea-env
>   # 如果不存在依赖包，则需要下载下载的依赖包环境，才能激活建立的虚拟环境
>   sudo apt-get install python3-venv
>   cd typeidea-env
>   source bin/activate
>   # 退出虚拟环境
>   deactivate
>   # 安装项目需求包
>   pip3 install xxx
>   ```

> ```python
> # Windows环境中使用python虚拟环境并激活
> # 创建
> virtualenv xxx-venv
> # 激活
> cd xxx-venv
> cd Scripts
> activate
> ```

**2.拆分settings**

>   ```python
>   # 拆分玩settings后要通过修改manage.py和wsgi.py文件让django知道settings文件的路径
>   profile = os.environ.get('TYPEIDEA_PROFILE','develop')
>   os.environ.setdefault("DJANGO_SETTINGS_MODULE", "typeidea.settings.%s"%profile)
>   ```
>

**3.使用git注意**

>   ```python
>   # 第一次上传
>   git init
>   # git add README.md
>   git commit -m "first commit"
>   git remote add origin https://github.com/somebady111/Typeidea.git
>   git push -u origin master
>   
>   # 第二次上传
>   git add *
>   git remote add origin https://github.com/somebady111/Typeidea.git
>   git push -u origin master
>   
>   # 分支
>   # 查看当前分支
>   git branch
>   # 建立分支
>   git cheakout [branch_name]
>   git cheakout -b [branch_name]
>   # 分支合并,先切换到要更新到的分支里再合并其他分支
>   git merge [branch_name]
>   # 删除分支
>   git branch -d [branch_name]
>   
>   # 远程
>   # 查看当前连接远程
>   git remote -v
>   # 删除远程连接
>   git remote rm [远程主机名]
>   # 删除远程分支:
>   git push origin --delete tar
>   
>   # 历史版本
>   # 回到之前较新的版本
>   # 查看历史版本记录
>   git reflog 
>   # 回到某个操作之后的状态
>   get reset --hard [id]
>   ```

**4.从数据库到前台展示**

>   ```text
>   从URL传参到view视图层，view通过对象从数据库判断查询，将查询的值返回给前端内容，前端写架构来显示出值
>   ```
>

**5.Django的验证码小模块：**

>   <https://blog.csdn.net/qq_37648632/article/details/83149803>
>
>   pip install django-simple-captcha

**6.使用xadmin替换Django自带后台admin：**

>   https://blog.csdn.net/u014793102/article/details/80316335
>
>   https://www.cnblogs.com/shhnwangjian/p/6372503.html
>
>   `安装：pip install git+https://github.com/sshwsfc/xadmin@697a658`
>
>   `安装：pip install https://github.com/the5fire/django-xadmin/archive/0.6.1.tar.gz`

**7.Linux安装mysql的方法：**

>   https://jingyan.baidu.com/article/a378c9609eb652b3282830fd.html

**8.实现搜索功能：**

>   1.写view视图函数（重写数据源，展示关键词），理解F和Q的功能，执行的是sql的查询语句
>
>   2.配置url的内容，用到revese反向通过name关联视图
>
>   3.修改搜索部分模板，抽离出公共部分的base.html中

**9.对渲染的理解：**

>   1.从具体的形式上就是把具体的代码段展示成用户可操作的界面
>
>   2.模板，view视图层，url
>
>   3.Django模板的自动转码功能

**10.富文本编辑器：**

>   <https://www.jianshu.com/p/65d2a748634b>
>
>   0.百度：django-ckeditor
>
>   1.安装：pip install django-ckeditor==5.4.0
>
>   2.配置：app
>
>   3.配置form中文本编辑方式
>
>   4.配置功能和皮肤参数

**11.Django轻量级的自动补全插件：**

>   <https://django-autocomplete-light.readthedocs.io/en/master/tutorial.html#>
>
>   pip install django-autocomplete-light==3.2.10

**12.前端js，css渲染时，文件位置：**

>   `1.防止浏览器在加载JavaScript时内容停止渲染，<script>一般放在页面底部`
>
>   2.根据实际的应用场景来合理配置资源放置位置
>
>   3.通过Markdown插件渲染到页面上：
>
>   ​		`1.通过Markdown中的代码，被渲染到<pre><code></code></pre>标签中`
>
>   ​		`2.前端代码高亮的库提取<code>中的代码，按照不同标签进行打包，再通过css展示出不同颜色`

**13.分布式内存的缓存的系统：**

>   <https://www.runoob.com/memcached/memcached-tutorial.html>

**14.小总结：**

>   1.建立项目架构
>
>   ​		1.python虚拟环境
>
>   ​		2.数据库设计，app的设计，必备的文件，配置git
>
>   ​		3.django的基础配置
>
>   2.理解ORM（model和数据库之间的架构）
>
>   3.管理后台（xadmin代替django自带的admin）
>
>   4.模板层，admin如何抽出基类
>
>   5.view视图和URL及templates之间的参数传递；模板查询的方式
>
>   6.前端使用bootstrap架构的写法
>
>   7.第三方插件的使用xadmin增强管理后台

**15.cached_property**

>   1.django提供的cached_property将返回的数据绑定到实例上
>
>   @cached_property
>
>   def xxx():
>
>   ​	return xxx

**16.通过shell进行测试：**

>   ```python
>   # 使用django-autocomplete-llight优化性能
>   # 首先进入shell
>   python manager.py shell
>   
>   # 创建测试
>   from blog.models import Caegory
>   from django.contrib.auth.models import User
>   user = User.objects.bulk_create([
>       Category(name='cate%s'%i,owner=user)
>       for i in range(1000)
>   ])
>   ```
>
>   在对性能化的优化上，使用django-autocomplete-light自动补全插件优化

**17.文本编辑器双选择，共生存：**

>   1.页面加载时，判断Markdown语法复选框是否被选中
>
>   2.如果选中展示Markdown编辑器，如果没选中，展示django-CKeditor编辑器
>
>   3.Markdown语法复选框点击事件负责监听，根据是否点击，判断编辑器是否被切换

**18.django-rest-framework插件：变现层转化**

>   1.安装：pip install django-rest-framework==3.8.2
>
>   2.接口配置包:pip install coreapi==2.3.3
>
>   3.接口文档API docs：from rest_framework.documentation import include_docs_urls
>
>   4.为不同的接口定义不同的serializer

**19.调试优化：**

>   1.print ；输出json和dict格式的数据使用pprint模块的pprint函数
>
>   2.logging
>
>   3.pdb 跟踪程序执行流程：
>
>   ```python
>   # 先运行程序，再进入pdb模式查看运行工作
>   import pdb 
>   pdb.set_trace()
>   # 进入pdb模式
>   python -m pdb <name>
>   ```
>
>   4.编译器打断点
>
>   5.timer程序测时间，配合装饰器一起使用
>
>   6.profile/cProfile性能测试工具
>
>   7.panel测试工具插件
>
>   ```python
>   # 安装djdt_flamegraph火焰图
>   pip install djdt_flamegraph==0.2.12
>   # 在config/models.py中设置SIderBar模型的get_all函数中增加sleep代码
>   # 通过 python manage.py runserver --noreload --nothreading启动项目
>   ```
>
>   8.pympler内存占用分析
>
>   ```python
>   # 安装
>   pip install pympler==0.5
>   # 配置settings/develop.py
>   ```
>
>   9.line_profiler行级性能分析插件
>
>   10.silk测试工具

**20.django的缓存模块：**

>   1.cache：from django.views.decorators.cache import cache_page
>
>   2.缓存配置:local-memory caching;  filesystem caching;  database caching;  memcached