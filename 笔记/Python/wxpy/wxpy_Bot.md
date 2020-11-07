# wxpy_Bot

- 简介

  - wxpy_Bom机器人对象（微信）

    - <https://wxpy.readthedocs.io/zh/latest/bot.html>

  - content

    - 机器人对象

      - from wxpy import *

    - 初始化和登录

      - bot = Bot()

      - 参数

        - cache_path	
          - 设置当前会话的缓存路径，并开启缓存功能；为 None (默认) 则不开启缓存功能。
          - 开启缓存后可在短时间内避免重复扫码，缓存失效时会重新要求登陆。
          - 设为 True 时，使用默认的缓存路径 ‘wxpy.pkl’。
        - console_qr
          - 在终端中显示登陆二维码，需要安装 pillow 模块 (pip3 install pillow)。
          - 可为整数(int)，表示二维码单元格的宽度，通常为 2 (当被设为 True时，也将在内部当作 2)。
          - 也可为负数，表示以反色显示二维码，适用于浅底深字的命令行界面。
          - 例如: 在大部分 Linux 系统中可设为 True 或 2，而在 macOS Terminal 的默认白底配色中，应设为 -2。
        - qr_path
          - 保存二维码的路径
        - qr_callback
          - 获得二维码后的回调，可以用来定义二维码的处理方式，接收参数: uuid, status, qrcode
        - login_callback
          - 登陆成功后的回调，若不指定，将进行清屏操作，并删除二维码文件
        - logout_callback
          - 登出时回调

      - 获取用户Id puid属性

        - ```python
          bot.enable_puid('wxpy_puid.pkl')
          ```

        - 是 **wxpy 特有的聊天对象/用户ID**不同于其他 ID 属性，**puid** 可始终被获取到，且具有稳定的唯一性

      - 为 True 时，将自动消除手机端的新消息小红点提醒 (默认为 False)

        - `Bot.auto_mark_as_read`

    - 获取聊天对象

      - bot.self

      - 给自己发送消息

        - 先用机器人添加自己为好友
          - bot.self.add()
          - bot.self.accept()
        - 发送消息给自己
          - bot.self.send('message')
        - 发送给文件助手
          - bot.file_helper()

      - 获取所有好友

        - bot.friends(update=False)

          

        - | 参数:     | **update** – 是否更新                                        |
          | :-------- | ------------------------------------------------------------ |
          | 返回:     | 聊天对象合集                                                 |
          | 返回类型: | [`wxpy.Chats`](https://wxpy.readthedocs.io/zh/latest/chats.html#wxpy.Chats) |

      - 获取所有的公众号

        - bot.mps(update=False)

          

        - | 参数:     | **update** – 是否更新                                        |
          | :-------- | ------------------------------------------------------------ |
          | 返回:     | 聊天对象合集                                                 |
          | 返回类型: | [`wxpy.Chats`](https://wxpy.readthedocs.io/zh/latest/chats.html#wxpy.Chats) |

      - 获取所有群聊信息

        - bot.groups(update=False,contact_only=False)

          

        - | 参数:     | **update** – 是否更新 **contact_only** – 是否限于保存为联系人的群聊 |
          | :-------- | ------------------------------------------------------------ |
          | 返回:     | 群聊合集                                                     |
          | 返回类型: | [`wxpy.Groups`](https://wxpy.readthedocs.io/zh/latest/chats.html#wxpy.Groups) |

      - 获取所有聊天对象

        - bot.chats(update=False)

          

        - | 参数:     | **update** – 是否更新                                        |
          | :-------- | ------------------------------------------------------------ |
          | 返回:     | 聊天对象合集                                                 |
          | 返回类型: | [`wxpy.Chats`](https://wxpy.readthedocs.io/zh/latest/chats.html#wxpy.Chats) |

    - 搜索聊天对象

    - 加好友建群

    - 控制多个微信

    - 聊天对象

    - 其他

- 具体见网址