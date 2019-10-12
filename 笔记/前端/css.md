**1.css资源的组织（内联和外联）**

内联css：写在头部的css标签样式

Django中静态资源的访问：settings.py中INSTALLED_APPS中增加`django.contrib.staticfiles`（默认的静态文件访问配置），仅限于在DEBUG=True 的情况下

Django静态资源在Nginx下的配置：当DEBUG=False后，静态资源需要根据Nginx进行配置，根据静态资源的配置模板寻找路径