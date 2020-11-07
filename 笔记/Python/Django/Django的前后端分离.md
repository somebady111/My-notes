# Django的跨域和前后端分离

**1.Django解决跨域的方法：**

> 解决django后端跨越问题的方法：
>
> ​	1.pip3 install django-cors-headers
>
> ​	2.配置settings：
>
> ​		在INSTALLED_APPS中添加模块corsheaders
>
> ​		ALLOWED_HOSTS = ['*']，如果前后端分离只允许前端的地址进行访问，加入内网IP
>
> ​	3.调用：
>
> ```python
> from corsheaders.defaults import default_headers
> CORS_ALLOW_HEADERS = default_headers+('ContentType',)
> CORS_ORIGIN_ALLOW_ALL = True
> CORS_ALLOW_CREDENTIALS = True
> ```
>
> ​	4.加拦截
>
> ​		`在中间键MIDDLEWARE中加corsheaders.middleware.CorsMiddleware`

**2.Django的前后端分离的方法：**

> ```python
> from django.conf.urls import url,include
> # 前后端分离调用模块
> from django.views.generic import TemplateView
> from django.contrib import admin
> 
> urlpatterns = [
>     url(r'^admin/', admin.site.urls),
>     url(r'^user/', include('userinfo.urls')),
>     url(r'^stock/', include('stocks.urls')),
>     url(r'^deal/', include('deal.urls')),
>     # url(r'^pay/', include('pay.urls')),
>     # 实现前后端分离
>     url(r'^index-1', TemplateView.as_view(template_name="index-1.html"), name='index'),
>     url(r'^header', TemplateView.as_view(template_name="header.html"), name='header'),
>     url(r'^index-2', TemplateView.as_view(template_name="index-2.html"), name='index'),
>     url(r'^index-3', TemplateView.as_view(template_name="index-3.html"), name='index'),
>     url(r'^index-4', TemplateView.as_view(template_name="index-4.html"), name='index'),
> 
> ]
> ```
>
> 

