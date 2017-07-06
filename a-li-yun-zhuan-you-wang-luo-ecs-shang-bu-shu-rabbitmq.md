# 阿里云专有网络ECS上安装rabbitmq说明



1.rabbitMq需要erlang语言的支持，输入 `sudo apt-get install erlang-nox` 安装erlang

2.输入 `sudo apt-get update` 更新apt

3.执行 `apt-get install rabbitmq-server  `

4.执行 `service rabbitmq-server start ` 测试

5.安装 RabbitMQWeb管理插件

    rabbitmq-plugins enable rabbitmq_management
    service rabbitmq-server restart

6.在操作系统中开放端口访问
* 执行 `ufw enbale`选择yes
* 执行 `ufw allow 15672/tpc`关闭服务器防火墙

7.在阿里云管理控制台中开放阿里云端口访问

* 进入阿里云管理控制台的云服务器ECS
* 点击实例进入后再点击更多，选择安全组配置进入
* 进入配置规则
* 点击添加安全组规则
* 填写如图内容

    ![](http://ww1.sinaimg.cn/mw690/006CEVoWgy1fgph369ikvj30as0bmq3t.jpg)
* 最后点击确认

8.使用`netstat -ntlp |grep 15672`测试是否开启正常端口

9.在网页上输入 `[服务器地址]:15672` 访问，如果出现下图所示网页则成功
    
![](http://ww1.sinaimg.cn/mw690/006CEVoWgy1fgpgyf40mrj30bm0a8mxb.jpg)
