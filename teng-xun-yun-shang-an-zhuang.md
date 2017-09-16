# 腾讯云上安装rabbitmq说明

1.rabbitMq需要erlang语言的支持，输入 `sudo apt-get install erlang-nox` 安装erlang

2.输入 `sudo apt-get update` 更新apt

3.执行 `apt-get install rabbitmq-server`

4.执行 `service rabbitmq-server start` 测试

5.安装 RabbitMQWeb管理插件

```
rabbitmq-plugins enable rabbitmq_management
service rabbitmq-server restart
```



7.在腾讯云管理控制台中开放阿里云端口访问



8.使用`netstat -ntlp |grep 15672`测试是否开启正常端口

9.在网页上输入 `[服务器地址]:15672` 访问，如果出现下图所示网页则成功

![](http://ww1.sinaimg.cn/mw690/006CEVoWgy1fgpgyf40mrj30bm0a8mxb.jpg)

