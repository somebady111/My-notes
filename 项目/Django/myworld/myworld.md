# MTV-model

## personal_info APP

**1.注册**表（register）

```reStructuredText
# 用户表设计
user(超级管理员账户)
nickname
username
password
status（注册状态），已注册，未注册
register_time（注册时间）
```

**2.注册登录**

```python
# MTV 架构

URL层——>请求View层——>请求到模板展示页面Templates——>通过Model层读取数据（此时model层已经建立数据库）并展示到HTML页面中——>

I.注册表：通关用户界面输入将注册信息写入数据库 II.登录：查询登录用户是否存在 III.personal_info相关内容：注册用户、用户登录、用户界面显示、用户信息更改和查询

# 注册页面的内容要关联到数据库中(数据传递)

# 登陆失败-弹出弹框（您输入的账户名或密码不正确，请先注册或检验您的身份信息）
# 登陆成功，跳转到新的页面，并显示当前登陆的用户名信息

# 插入 更改 两个查询数据库
# 新的页面:登陆后的我的商城 我的帖子页面，先打印出一行提示信息出来（登陆成功），在解决后期的页面跳转问题

# view层通过model层去读取数据库的数据
# 接口1：all（）
users = Register.objects.all() # 查询数据库所有内容	
users = Register.objects.filter(username='jing') # 根据条件过滤查询数据库
user = Register.objects.get(username='z') # 查找对象的指定属性

# templates层通过参数，传参到url，url再请求到view层

# 接收模板templates的输入结果，并交给view层和model层检验

# 攻坚django登录系统的尝试办法
'''
1.表单请求
2.view层到model层再进入数据库的查询
3.根据请求方式返回页面内容
4.根据查询的用户名来指定搜索对应的用户密码
'''
# 攻坚1：表单请求
# 代码段：TEMPLATES层
<form action="" method="POST">
    {% csrf_token %}
    <P>
    username:<input type="text" name="username" maxlength=10 placeholder="请输入用户名">
    </P>
    <p>
    password:<input type="password" name="password" maxlength=15 placeholder="请输入账户密码">
    </p>
    <p>
    <input type="checkbox">自动登录
    </p>
    <p>
    <button type="reset" value="">重置</button>
    <button type="submit" value="">登录</button>
    </p>
    <a href="register">立即注册</a>
</form>

# 代码段：vies层
def Login(request):
    '''
    这是登录界面对数据库的查询操作
    :param request:
    :param username: 用户名
    :param password: 用户密码
    :return: 查询到的用户名和用户密码
    '''
    form = Form(request.POST)
    # 攻坚3
    if request.method == "GET":
        return render(request, 'personal_info/login.html')

    elif request.method == "POST":
        if form.is_valid():
            # 攻坚2
            username = form.cleaned_data['username']
            password = form.cleaned_data['password']

            try:
                user = Register.objects.get(username=username).username
                # 攻坚4
                passwd = Register.objects.get(username=user).password
                if user == username and passwd == password:
                    return HttpResponse('输入账号密码正确')

            except Register.DoesNotExist:
                return HttpResponse('warning_info:{}'.format('输入账号密码错误'))
            else:
                return render(request, 'personal_info/login.html', context={'form':form})
            

# 第一阶段任务
# 1.登录 2.注册 3.修改账户信息 4.个人信息展示
# 11-10 完成进度：登录，显示登陆后信息页面跳转

# 第一阶段 11-11：注册页面逻辑编写
# 思路：获取表单请求，插入数据库中
# 代码
def register_user(request):
    '''
    这是对model层建立数据库的插入操作
    :param request:
    :param nickname: 昵称
    :param username: 用户名
    :param password: 用户密码
    :return: 插入数据库信息
    '''
    try:
        form = Form_2(request.POST)
        # 判断请求方式,第一层请求到页面，不提交任何内容，只返回给页面，提交方式为GET
        if request.method == "GET":
            return render(request, 'personal_info/register.html')

        elif request.method == "POST":
            # 表单校验
            if form.is_valid():
                try:
                    # 表单数据
                    nickname = form.cleaned_data['nickname']
                    username = form.cleaned_data['username']
                    password = form.cleaned_data['password']
                    repassword = form.cleaned_data['repassword']

                    # 判断密码是否一致
                    if password == repassword:
                        # 插入数据到数据库
                        Register.objects.create(nickname=nickname, username=username, password=password)
                        return HttpResponse('添加用户成功')
                    else:
                        return HttpResponse('您输入的密码不一致')

                except:
                    return HttpResponse('添加用户失败,可能您输入的用户名或昵称已经重复')
    except:
        return HttpResponse('请输入正确的内容！')
    

# 11-16 错误总结：关于静态文件不能正确读取的修正方法
# 1.在settings.py（base.py）中设置静态文件地址
# 静态资源存放目录
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR,'themes',THEME,"static"),
]
STATIC_ROOT = '/tmp/static'
# 在HTML中引用静态地址内容
<link rel="shortcut icon" href="../static/image/favicon.png" type="image/x-icon">
<link rel="stylesheet" href="../static/css/personal_info.css">
```



**3.页面报告详情**

```reStructuredText
# log.html 起始页面
1.登录界面
2.注册导航
3.起始页展示

# login.html 登录页面
1.个人登录名显示
2.商城、帖子、今日热点等页面展示（主体内容）

# register.html 注册页面
1.注册信息
2.注册

# personal_infomation.html 个人信息页面
1.个人信息
2.更改个人信息链接

# 遗留问题
1.用户名和账户密码为空时，提交POST请求，反馈ValueError错误		

# 11-11:任务总结：
# 完成的
1.完成register逻辑编写
# 待解决
1.注册端如果用户名一致，则重新输入
2.完善整个返回页面：js弹窗
3.超级用户表重新建立，统一管理所有注册用户（这里的User关联外键因为起到不能为空的场景）

# 11-12：session
1.更新个人信息
2.个人信息展示（需要一个模板继承，继承自一个共有数据的共有模板）
3.添加JS事件：点击事件弹框、对输入字段长度检验
4.抽取模板（log.html、login.html、personal.html、modify_personal.html这三个模板公用的登录信息抽取，并被继承）
5.抽取css，设置JS
6.19-12-23解决方案：使用类对对views层进行类继承使得当前登录用户可以公共调用
# 点击事件返回节点统计(JS)
1.登录错误弹出提示弹框
2.注册按钮提示的错误弹框
# 共享个人信息模板
1.位置：base.html
2.所有的登录、展示、个人信息展示都继承自base.html
3.查看base.html中当前登录用户信息获取方式		 
4.个人信息展示、更改占时搁置
```



## GOODS APP

**1.goods商品表**

```reStructuredText
id
name
price
description
```



## 帖子 APP

**1.categoty分类表**

```reStructuredText
id
name
status
created_time
is_nav
posts
owner
```

**2.title标题表**

```reStructuredText
id 
name
status
created_time
posts
owner
```

**3.siderbar侧边栏**

```reStructuredText
id
title
type
status
content
created_time
posts
owner
```

**4.post标签表**

```reStructuredText
id
title
description
content
status
created_time
updated_time
category
tags
comments
owner
```

**5.comment评论表**

```reStructuredText
id
nickname
email
website
content
created_time
post
```

**6.link友链表**

```reStructuredText
id
title
href
status
created_time
weight
owner
```
