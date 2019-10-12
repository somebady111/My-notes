# python虚拟化Windows和Linux环境的使用方法

**Linux环境：**

> `安装python虚拟环境包：`
>
> ```python
> # 现在Ubuntu中安装虚拟环境插件
> sudo apt install virtualenv
> # 安装虚拟环境包到系统目录
> pip3 install virtualenv
> # 安装虚拟环境宝到用户目录
> pip3 install virtualenv --user
> ```
>
> `创建虚拟环境：`
>
> ```python
> # 创建名为project-env的虚拟环境
> virtualenv project-env
> ```
>
> `激活创建的虚拟环境：`
>
> ```text
> cd project-env
> source bin/activate
> ```
>
> `备注：`
>
> ```text
> 如果执行成功，会进入到如下的目录下：
> (project-env) jingzuo@ubuntu:~/project-env$ 
> ```
>
> `安装项目需求库：`
>
> ```python
> # 这里以Django为例
> pip install django~=1.11
> ```
>
> `备注：`
>
> ```text
> 然后就可以创建项目目录了
> ```
>
> 
>
> 