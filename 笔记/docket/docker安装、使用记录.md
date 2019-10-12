# docker安装、使用记录

**1.环境：**

Linux

**2.作用：**

1.打包环境，生成docker

**3.安装：**

1.阿里云官网安装地址：<https://help.aliyun.com/document_detail/60742.html>

**4.使用方法：**

<https://www.cnblogs.com/zhanghongfeng/p/8067861.html>

```python
Options:
      --add-host list           Add a custom host-to-IP mapping (host:ip)
      --build-arg list          Set build-time variables
      --cache-from strings      Images to consider as cache sources
      --cgroup-parent string    Optional parent cgroup for the container
      --compress                Compress the build context using gzip
      --cpu-period int          Limit the CPU CFS (Completely Fair
                                Scheduler) period
      --cpu-quota int           Limit the CPU CFS (Completely Fair
                                Scheduler) quota
  -c, --cpu-shares int          CPU shares (relative weight)
      --cpuset-cpus string      CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string      MEMs in which to allow execution (0-3, 0,1)
      --disable-content-trust   Skip image verification (default true)
  -f, --file string             Name of the Dockerfile (Default is
                                'PATH/Dockerfile')
      --force-rm                Always remove intermediate containers
      --iidfile string          Write the image ID to the file
      --isolation string        Container isolation technology
      --label list              Set metadata for an image
  -m, --memory bytes            Memory limit
      --memory-swap bytes       Swap limit equal to memory plus swap:
                                '-1' to enable unlimited swap
      --network string          Set the networking mode for the RUN
                                instructions during build (default "default")
      --no-cache                Do not use cache when building the image
      --platform string         Set platform if server is multi-platform
                                capable
      --pull                    Always attempt to pull a newer version of
                                the image
  -q, --quiet                   Suppress the build output and print image
                                ID on success
      --rm                      Remove intermediate containers after a
                                successful build (default true)
      --security-opt strings    Security options
      --shm-size bytes          Size of /dev/shm
      --squash                  Squash newly built layers into a single
                                new layer
      --stream                  Stream attaches to server to negotiate
                                build context
  -t, --tag list                Name and optionally a tag in the
                                'name:tag' format
      --target string           Set the target build stage to build.
      --ulimit ulimit           Ulimit options (default [])
```

```python
选项：

--添加主机列表添加自定义主机到IP映射（主机：IP）

--生成参数列表设置生成时间变量

--从字符串图像缓存以将其视为缓存源

--cgroup父字符串容器的可选父cgroup

--压缩使用gzip压缩生成上下文

--CPU周期int限制CPU CFS（完全公平

调度）时段

--CPU配额int限制CPU CFS（完全公平

调度程序）配额

-c，-cpu共享int cpu共享（相对权重）

--cpuset cpus允许执行的字符串cpu（0-3，0,1）

--cpuset mems允许执行的字符串mems（0-3，0,1）

--禁用内容信任跳过图像验证（默认为真）

-f，-dockerfile的文件字符串名称（默认为

'path/dockerfile'）

--强制RM始终移除中间容器

--iidfile字符串将图像ID写入文件

--隔离串容器隔离技术

--图像的标签列表集元数据

-M，--内存字节内存限制

--内存交换字节交换限制等于内存加交换：

“-1”启用无限制交换

--网络字符串设置运行的网络模式

生成期间的说明（默认为“默认”）。

--无缓存在生成图像时不使用缓存

--如果服务器为多平台，则设置平台字符串

能干的

--拉总是尝试拉新版本的

图像

-Q，-quiet抑制生成输出和打印图像

成功时的ID

--rm在a之后移除中间容器

成功生成（默认为真）

--安全选项字符串安全选项

--shm大小/dev/shm的字节大小

--将新构建的层挤压成单个

新层

--流流连接到服务器进行协商

生成上下文

-t，--标记列表名称和可选的标记

'name:tag'格式

--目标字符串将目标生成阶段设置为生成。

--ulimit ulimit ulimit选项（默认值[]）
```

**5.创建一个duckerfile**

```python
# 首先确保安装好ducker
# 在项目根目录创建一个requirements.txt文件，文件中列出所有需求的运行环境，如果需要特定的版本号，要特定指出
# 在项目目录下创建duckfile文件
# 第一行写 ducker的基础镜像
# 第二行写添加到环境变量中
ADD . /code # 添加到虚拟容器中
# 第四行写项目工作目录
RUN pip3 install requirements.txt  # 第五行运行requirements文件
CMD scrapy crawl quotes  # 运行项目命令
# 如果项目配置有本地数据库，需要将数据库配置到远程数据库中
```

**6.构建镜像**

```python
docker build -t quotes:latest .
```

**7.检查生成镜像**

```python
docker images
```

**8.运行镜像**

```python
docker run quotes
```

**9.推送至docker hub**

```python
# 先在docker官网注册账号xxxx，新建一个response取名为xxx
# 为新建的镜像打一个标签
docker tag quotes:latest xxxx/xxx:latest
# 推送
docker push xxxx/xxx
```

**10.在其他服务器上获取到docker**

```python 
# 安装好docker
docker run xxxx/xxx
```

