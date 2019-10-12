# mitmproxy-得到

**1.环境：**

1.Windows、得到APP

**2.问题：**

1.sslv3证书配置

2.经过测试的网站不支持TLSv1.2。一旦mitmproxy发送CHELO消息，服务器就会发回FIN消息并中断连接。

