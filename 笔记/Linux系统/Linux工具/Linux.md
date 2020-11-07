# Linux

- **简介**

  - 首先Linux是一个操作系统，有他自己的一个操作模式，于其并列的两个两个著名的操作系统是Unix和Windows

- 编外链接

  - [点此处查看其他帖子详细介绍](https://mp.weixin.qq.com/s?__biz=MzU3NTgyODQ1Nw==&mid=2247485633&idx=1&sn=0aea982487c6e3118c6a67e1941ba8c6&chksm=fd1c7047ca6bf9518d619a503896d1282df45df195542b112e6bcf3e9fa6bdbe0cb92bbd99ef&scene=0&xtrack=1&key=906dea2828a54f86aa48052adbaca4eae4af73026b50ff076661dcdd4c513e6eecc813c7fd4b27cacb05d5632766426992a3b45e7ca85c099508924680fd617160db7443583e6e77cab9163de611571e&ascene=14&uin=MTI3MDAzMzE3Ng%3D%3D&devicetype=Windows+10&version=62060739&lang=en&pass_ticket=a4Rt1CK%2Fq6NddbuStFwPKkUf1h61AvHsVUQCJKvnje4DuQuMDBIhqqKS0SBprjsq)
  - [点此处查看Linux用户权限管理操作](https://cloud.tencent.com/developer/article/1152693)

- **Linux的版本**

  - Ubuntu
  - Redhat
  - CentOS
  - Android

- **基础的命令操作**

  - 帮助键

    - > 命令名 --help
      >
      > tree
      >
      > - 显示目录树

  - 查（查询当前系统的存在的文件）

    - > ls  
      >
      > - 查看当前位置文件夹
      >
      > ls-a 
      >
      > - 显示全部文件夹
      >
      > ls-l 
      >
      > - 显示当前文件夹的详细信息
      >
      > pwd 
      >
      > -  显示当前的工作路径
      >
      > find
      >
      > - find 路径 -name "文件名"
      > - find . ( -name "*.txt" -o -name "*.pdf" ) -print
      > - 根据文件名信息查找指定的文件
      >
      > find用正则方式查找
      >
      > - find . -regex  ".*(.txt|.pdf)$"
      >
      > grep
      >
      > - grep "内容" 【选项】 文件名或路径
      >   - -n  显示行号
      >   - -r 递归搜索文件夹内的文件
      > - 查找文件中的相应内容及文本信息
      >
      > - grep "文件名" -nr 路径（递归搜索全部内容）

  - 路径

    - > .  
      >
      > - 当前位置
      >
      > .. 
      >
      > - 上一层位置
      >
      > cd 【目录名】 
      >
      > - 切换目录位置

  - 增

    - 用命令创建一个文件

      - > mkdir 文件名

    - 用命令创建一个文件夹

      - > touch 文件夹名
        >
        > - touch的文件夹名存在则更改系统文件夹创建的时间
        >
        > - touch的文件名不存在则创建该文件夹
        >
        > cp【选项】 源文件或文件夹 目标文件或文件夹
        >
        > - cp -a 复制文件夹里的内容形成另一个文件

  - 删

    - 用命令删除一个空文件夹

      - > rmdir 文件夹名

    - 用命令删除文件或文件夹

      - > rm -r 
        >
        > - 递归删除文件夹内部的文件或文件夹
        >
        > rm -i 
        >
        > - 删除前给出提示
        >
        > rm -f 
        >
        > - 强制删除，不给出提示
        >
        > mv 源文件或文件夹 目标文件文件夹
        >
        > - 文件的转移形成另一个文件或仅仅是转移

  - 改

  	- 命令rm xxx.py xxxx.py

  - 压缩和解压缩
  
    - 压缩

      - > gzip 文件名
      >
        > - 用zip压缩算法对文件进行压缩，生成压缩后的.gz文件
  
    - 解压缩

      - > gunzip 文件名
      >
        > - 对zip压缩算法的文件进行解压

  - 打包和解包
  
    - 打包和解包
  
      - > tar 【选项】 文件名 【文件名或路径】
        >
        > - -czvf 文件名 
        >   - 创建包并进行压缩
        > - -xzvf 文件名 
        >   - 解压包到当前文件夹下
        >
      > 示例：
        >
      > - tar-czvf 包名.tar.gz 文件夹名
        > - tar-xzvf 包名.tar.gz
  
  - 权限操作
  
    - > sudo【选项】 【参数】
      >
    > - -i 
      >   - 切换到root用户
    > - exit
      >   - 退出用户登录
  
  - 添加用户

    - > sudo adduser test
    >
      > - 会自动将该用户添加到同名组中，创建/home/test/，从etc/skel/复制文件，并设定密码和相关初始身份信息。

  - 让用户获得root权限
  
    - 修改/etc/sudoers 文件，找到下面一行，在root下面添加一行：
  
    > Allow root to run any commands anywhere
      >
    > root ALL=(ALL) ALL
      > test ALL=(ALL) ALL

    - 然后修改用户，使其属于root组，命令如下：

      > adduser test root
  
- 删除用户：
  
  - sudo userdel test
    - rm -rf /home/test
  
  - 切换用户
  
    - > su [参数] [-] [用户名]
      >
      > - su test #切换到test用户
      > - su #切换到root用户
      > - 可以使用su命令来切换用户，su是switch user切换用户的缩写。可以是从普通用户切换到root用户，也可以是从root用户切换到普通用户。从普通用户切换到root用户需要输入密码，从root用户切换到普通用户不需要输入密码。

