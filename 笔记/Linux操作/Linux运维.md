# Linux运维

**1.scp：**

> `scp指定端口传输文件：`
>
> scp -P 27 Magnifique_fond_ecran_HD_-_Hiver.jpg  root@bystart.cn:/home/project 
>
> `ssh指定端口连接：`
>
> ssh -p 27 root@bystart.cn

**2.关闭Linux已占用的端口号：**

1.查看被占用的端口号：netstat -tln | grep PORT

2.查看被占用端口的PID：sudo lsof -i:PORT

3.关闭进程：sudo kill -9 PID

**3.命令用法：**

```python 
# 创建settings文件夹，并在settings中创建__init__.py文件
mkdir settings && touch settings/__init__.py
# 将settings.py文件移动到settings文件夹，并改名为base.py
mv settings.py settings/base.py
```

**4.重启网卡**

```python
/etc/init.d/networking restart
```



