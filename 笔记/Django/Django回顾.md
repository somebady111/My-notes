# 开始一个Django项目（个人帖子）

**0.创建一个Django项目：**

> django-admin startproject 项目名称

**1.创建APP：**

> cd 项目名称
>
> python manage.py startapp app名称

**2.配置APP：**

> `配置app（侧边栏，友链）`
>
> ​	1.友链（标题，链接，状态，权重，作者，创建时间）
>
> ​	2.侧边栏（标题，展示类型，内容，状态，作者，创建时间）
>
> `评论app`
>
> ​	1.评论（评论目标，评论内容，评论昵称，网站，邮箱，状态，创建时间）
>
> `内容app（1.文章2.标签3.分类）`
>
> ​	1.分类（分类名称，作者，创建时间，是否导航，状态）
>
> ​	2.标签（标签名称，作者，状态，创建时间）
>
> ​	3.文章（标题，摘要，正文，状态，分类，标签，作者，创建时间）

**3.在settings中配置APP：**

> 修改settings.py/base.py中INSTALLED_APPS的内容

**4.创建数据库：**

> 如果是配置在jango内部的sqlites数据库，直接运行manage.py文件
>
> 如果配置在外部自己创建的mysql数据库，修改settings.py中

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'stock11_db',
        'USER':'root',
        'HOST':'localhost',
        'PORT':3306,
        'PASSWORD':'jing19961219',
    }
}
```

```python
# 添加调用python的mysql方式在__init__.py中配置
import pymysql

pymysql.install_as_MySQLdb()
```

**5.权限文件修改：**

> vi 文件名
>
> nano 文件名
>
> gedit 文件名

**6.阶段：**

> Nginx到NGINX启动 缺少前端文件
>

**7.项目：**

> 后台注册，登录，验证逻辑（装饰器），登出
>
> 注册：
>
> ​	获取表单注册名
>
> ​	判断用户名，密码是否为空
>
> ​	判断用户名是否存在
>
> ​	判断两次密码是否输入一致
>
> ```python
> # 注册
> def register_(request):
>     # 判断请求方式
>     if requests..method = "POST":
>         new_user = UserInfo()
>         # 获取表单用户名
>         new_user.username = request.POST.get("username","")
>         if not new_user.username:
>             return HttpResponse(json.dumps({"result":False,"data":"","error":"用户名不能为空"}))
> 
>         # 判断用户名是否存在
>         # 查询用户名
>         olduser = UserInfo.objects.filter(username=new_user.username)
>         if olduser:
>             return HttpResponse(json.dumps({"result":False,"data":"","error":"用户名已存在"}))
> 
>         # 获取表单密码
>         password = request.POST.get("password","")
>         if not password:
>             return HttpResponse(json.dumps({"result":False,"data":"","error":"密码不能为空"}))
> 
>         # 密码确认验证
>         cpssword = request.POST.get("cpassword","")
>         if password != cpassword:
>             return HttpResponse(json.dumps({"results":False,"data":"","error":"两次密码不一致"}))
> 
>         # django密码加密方式
>         new_user.password = make_password(password,None,'pbkdf2_sha1')
>         new_user.uemail = request.POST.get("uemail","")
>         new_user.uphone = request.POST.get("uphone","")
>         new_user.save()
>         return HttpResponse(json.dumps({"result":True,"data":"注册成功","error":""}))
>     elif request.method == "GET":
>         return render(request,"register.html",{})
>         return HttpResponse(json.dumps({"result":False,"data":"","error":"提交方式错误"}))
>     
> ```
>
> 
>
> 登录：
>
> ​	获取表单内容
>
> ​	验证用户名密码是否存在，如果不存在给出提示
>
> ​	验证用户名是否激活，是否存在
>
> ```python
> def login_(request):
>     if request.method == "POST":
>         username = request.POST.get('username',"")
>         password = request.POST.get('password',"")
>         if not username:
>             return HttpResponse(json.dumps({"result": False, "data": "", "error": "用户名不能为空"}))
>         if not password:
>             return HttpResponse(json.dumps({"result": False, "data": "", "error": "密码不能为空"}))
>         user = authenticate(username=username,password=password)
>         if user is not None and user.is_active:
>             login(request,user)
>             url = request.COOKIES.get("source_url","")
>             return HttpResponse(json.dumps({"result":True,"data":{"url":url},"error":""}))
>         else:
>             return HttpResponse(json.dumps({"result":False,"data":"","error":"用户名密码错误"}))
>     elif request.method == "GET":
>         return HttpResponse(json.dumps({"result": False, "data": "", "error": "提交方式错误"}))
> 
> 
> ```
>
> 
>
> 充值：
>
> ​	判断是否登录
>
> ​	获取表单（充值钱，银行卡，密码）
>
> ​	如果银行卡存在，判断密码是否正确，不存在则报错	
>
> ```python
> auth_check = "helloworld"
> 
> def check_login_status(func):
>     def wrapper(request,*args,**kwargs):
>         if request.user.is_authenticated():
>             return func(request, *args, **kwargs)
>         else:
>             return HttpResponse(json.dumps({"result":False,"data":"","error":"未登录,用户不合法"}))
>     return wrapper
> 
> 
> @check_login_status
> def bandbank(request):
>     if request.method == "POST":
>         user = request.user
>         truename = request.POST.get("truename","")
>         bank = request.POST.get("bank","")
>         bankNo = request.POST.get("bankNo","")
>         tradepwd = request.POST.get("tradepwd","")
>         if user:
>             try:
>                 Bank.objects.create(user=user[0].id,truename=truename,bank=bank,bankNo=bankNo,tradepwd=tradepwd)
>             except ObjectDoesNotExist as e:
>                 logging.warning(e)
>         else:
>             return HttpResponse(json.dumps({"result":False,"data":"","error":"用户不存在"}))
> ```
>
> 
>
> 验证：
>
> ​	验证是否登录，采用装饰器验证
>
> ​	验证用户表中信息是否正确
>
> 登出：
>
> ​	登出函数，返回值
>
> ```python
> def logout_(request):
>     logout(request)
>     return HttpResponse(json.dumps({"result": True, "data": "注销成功", "error": ""}))
> 
> ```
>
> E-M图：
>
> 重置：
>
> 绑定卡：
>
> 信息展示：
>
