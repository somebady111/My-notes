1、运维概述
  1、什么是运维 ：服务器运行维护
  2、名词
    * IDC(互联网数据中心)
      机柜租赁、服务器租赁、服务器托管
    * 常用Linux操作系统
      RedHat(红帽)
      CentOS
      Ubuntu
    * 虚拟化(见图)
2、Linux常用命令
  1、ifconfig ：查看本机IP地址和MAC地址
  2、ping ：测试网络连通性,-c可指定次数
     * ping IP地址/域名
  3、nslookup ：解析域名对应的IP地址
     * nslookup www.baidu.com
  4、top ：Linux下任务管理器
  5、ps -aux ：显示系统进程命令(PID)
     * ps -aux | grep 'mysql'
  6、kill ：杀死一个进程
     * sudo kill 进程PID号
  7、df -h ：查看磁盘使用情况
     * df -h
  8、ls -lh ：l长格式,h提供易读容量单位
     * ls -lh 文件名
  9、chmod ：修改文件权限
     * ls -lh 文件名
     -rw-rw-r-- tarena tarena
                所有者 所属组
      r ：4(读)
      w ：2(编辑)
      x ：1(可执行)
      chmod 666 a.txt
            rw-r--r--
      chmod +x a.txt

    a.txt  rw-r----- tarena tarena
    组1 ：tarena
     用户1 ：tarena (rw)
     用户2 ：tarena2 (r)
     用户3 ：tarena3 (r)
    组2 ：clause
     用户1 ：clause1 (---)
     用户2 ：clause2 (---)
  10、wc -l ：统计文件行数
     * wc -l 文件名
  11、sort ：对文件中内容进行排序
     * sort 文件名
  12、uniq -c ：去除重复行,并统计每行出现的次数
     * sort ip.txt | uniq -c
  13、find ：查找指定文件
     * find 路径 -name 文件名
       sudo find / -name 'tessda*'
  14、ssh ：远程连接到服务器
     * ssh 用户名@IP地址
  15、scp ：远程复制(本地-服务器)
     * scp 文件名 用户名@IP地址:绝对路径
3、周期性计划任务
  1、进入周期性计划任务设置
     * crontab -e
  2、设置周期性计划任务
     5 2 15 * * python3 /home/tarena/backup.py
     分 时 日 月 周
     分 ：0-59
     时 ：0-23
     日 ：1-31
     月 ：1-12
     周 ：0-6
  3、示例
    * 每分钟执行一次backup.py
      * * * * * python3 /home/tarena/backup.py
    * 每隔10分钟去执行一次bcakup.py
      */10 * * * * python3 /home/tarena/backup.py
    * 0点到6点之间,每隔1个小时执行backup.py
      0 0-6/1 * * * python3 /home/tarena/backup.py
    * (仅限周末)每小时的05分和35分执行backup.py
     5,35 * * * 6,0 python3 /home/tarena/backup.py
  4、知识点
    * ：所有可能值
    / ：指定时间间隔单位
    - ：指定一个时间段
    , ：多个值之间用,隔开
4、awk使用(是一行一行处理)
  1、命令格式 ：awk 选项 '动作' 文件
  2、用法 ：Linux命令 | awk 选项 '动作'
  3、选项 ：-F '分隔符' 
     * 指定分隔符,默认以空白分割
  4、示例
    * 显示所有的文件系统(df -h)
      df -h | awk '{print $1}' # $1指第一列
    * 把当前Linux操作系统中,前10个用户名输出
      cat /etc/passwd | awk -F ':' '{print $1}' | head -10
    * 把本机的IP地址提取出来
      ifconfig | head -2 | tail -1 | awk '{print $2}' | awk -F ':' '{print $2}'
    * nginx访问日志目录：/var/log/nginx/access.log
      1、把访问过自己的IP地址输出
        awk '{print $1}' access.log 
      2、统计一共有多少个IP访问过我
        awk '{print $1}' access.log | sort | uniq | wc -l
      3、统计每个IP访问的次数,把访问次数最多的前10位用户的IP及次数输出
        awk '{print $1}' access.log | sort | uniq -c | sort -rn -k 1 | head -1
        * -r ：倒序排列
	* -n ：以数值方式排
	* -k ：按照第n列进行排序
	* -c ：统计



     


  
  

  
  
      















