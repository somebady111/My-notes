# logging

**python的logging模块**

- URL：<https://www.cnblogs.com/yangliheng/p/6058436.html>
  
- 程序实例：modul_2文件
  
- 提供函数
  - debug()
  - info()
  - warning(）
  - error() 
  - critical()
  - 日志级别：`debug<info<warning<error<critical`

- 记录日志

  - 创建mylogger，%(name)s可调用这个名字
    
  - mylogger = **logging.getLogger**('mylogger')
    
  - 设置级别

    - **mylogger.setLevel(logging.DEBUG)**

  - 定义日志输出格式formatter

    - formatter = **logging.Formatter**('%(asctime)s - %(name)s - %(filename)s- %(levelname)s - %(message)s')

  - 创建一个handler，用于写入日志文件,只输出debug级别以上的日志，并调用定义的输出格式

    - ```python
      log_file = '{}年{}月{}日{}时{}分.log'.format(time.strftime('%Y'),time.strftime('%m'), time.strftime('%d'),time.strftime('%H'), time.strftime('%M'))
      ```

    - fh = **logging.FileHandler**(log_file)

    - fh.**setLevel(logging.DEBUG)**

    - fh.**setFormatter(formatter)**

  - 给我们开始实例化的mylogger对象添加handler

    - **mylogger.addHandler(fh)**
    - mylogger.addHandler(ch)

  - 直接在本模块中调用记录两条日志——**生产环境会封装成函数调用**

    - **mylogger.info**('foorbar')
    - mylogger.debug('just a test ')

- 输出控制台

  - 再创建一个handler，用于输出到控制台, 一般不用
    - ch = **logging.StreamHandler()**
    - ch.setLevel(logging.DEBUG)
    - *ch.setFormatter(formatter)*

- 根据大小拆分mylogger

  - ```python
    import logging
    import logging.handlers
    LOG_FILE = 'api.log'
    ＃定义日志文件切割规则最多备份5个日志文件，每个日志文件最大10M
    handler = logging.handlers.RotatingFileHandler(LOG_FILE, maxBytes = 10*1024*1024, backupCount = 5)
    
    # handler=logging.handlers.TimedRotatingFileHandler(**LOG_FILE**, when='midnight') #每天零点切换
    
    ＃定义日志输出格式
    fmt = '%(asctime)s - %(filename)s:%(lineno)s - %(name)s - %(message)s'
    formatter = logging.Formatter(fmt) # 实例化formatter
    handler.setFormatter(formatter) # 为handler添加formatter
    # 实例化一个logger对象，并为其绑定handle
    # test为写在日志里的name
    mylogger = logging.getLogger('test')
    mylogger.addHandler(handler) # 为mylogger添加handler
    mylogger.setLevel(logging.DEBUG) # 为mylogger设置输出级别
    
    # 调用mylogger
    
    mylogger.info('first info message')
    mylogger.debug('first debug message')
    ```

- **format:** 指定输出的格式和内容，format可以输出很多有用信息，如上例所示:

  - **%(name)s** 打印logger名，默认为root
  - **%(levelno)s:** 打印日志级别的数值
  - **%(levelname)s**: 打印日志级别名称
  - **%(pathname)s:** 打印当前执行程序的路径，其实就是sys.argv[0]
  - **%(filename)s:** 打印当前执行程序名
  - **%(funcName)s:** 打印日志的当前函数
  - **%(lineno)d:** 打印日志的当前行号
  - **%(asctime)s:** 打印日志的时间
  - %(message)s: 打印日志信息
  - %(thread)d: 打印线程ID
  - %(threadName)s: 打印线程名称
  - %(process)d: 打印进程ID

- logging.config模块通过配置文件的方式，加载logger的参数——最好用的方式

  ```python
  # cat logger.conf
  
  # 定义logger模块，root是父类，必需存在的，其它的是自定义。
  
  # logging.getLogger(NAME)就相当于向logging模块注册了实例化了
  
  # name 中用 . 表示 log 的继承关系
  
  [loggers]
  keys=root,example01,example02
  
  # [logger_xxxx] logger_模块名称
  
  # level 级别，级别有DEBUG、INFO、WARNING、ERROR、CRITICAL
  
  # handlers 处理类，可以有多个，用逗号分开
  
  # qualname logger名称，应用程序通过 logging.getLogger获取。对于不能获取的名称，则记录到root模块。
  
  # propagate 是否继承父类的log信息，0:否 1:是
  
  [logger_root]
  level=DEBUG
  handlers=hand01,hand02
  [logger_example01]
  handlers=hand01,hand02
  qualname=example01
  propagate=0
  [logger_example02]
  handlers=hand01,hand03
  qualname=example02
  propagate=0
  
  # [handler_xxxx]
  
  # class handler类名
  
  # level 日志级别
  
  # formatter，上面定义的formatter
  
  # args handler初始化函数参数
  
  [handlers]
  keys=hand01,hand02,hand03
  [handler_hand01]
  class=StreamHandler
  level=INFO
  formatter=form02
  args=(sys.stderr,)
  [handler_hand02]
  class=FileHandler
  level=DEBUG
  formatter=form01
  args=('myapp.log', 'a')
  [handler_hand03]
  class=handlers.RotatingFileHandler
  level=INFO
  formatter=form02
  args=('myapp.log', 'a', 10*1024*1024, 5)
  
  # 日志格式
  
  [formatters]
  keys=form01,form02
  [formatter_form01]
  format=%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s
  datefmt=%a, %d %b %Y %H:%M:%S
  [formatter_form02]
  format=%(asctime)s%(name)-12s: %(levelname)-8s %(message)s
  datefmt=%a, %d %b %Y %H:%M:%S
  调用
  import logging
  import logging.config
  logging.config.fileConfig("logger.conf")
  logger = logging.getLogger("example01")
  logger.debug('This is debug message')
  logger.info('This is info message')
  logger.warning('This is warning message')
  生产环境中使用案例——通过函数定义调用
  A：定义到工具模块中
  cat util.py
  import logging,
  import logging.config
  def write_log(loggername):
  work_dir = os.path.dirname(os.path.realpath(__file__))
  log_conf= os.path.join(work_dir, 'conf/logger.conf')
  logging.config.fileConfig(log_conf)
  logger = logging.getLogger(loggername)
  return logger
  B: 外部模块调用
  cat test.py
  import util
  util.write_log('api').info('just a test')
  ```

- 

