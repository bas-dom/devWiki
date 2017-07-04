# 1 RabbitMQ简介
RabbitMQ是一个消息队列协议的实现，服务器端用Erlang语言编写，支持多种客户端，如：Python、Ruby、.NET、Java、JMS、C、PHP、ActionScript、XMPP、STOMP等，支持AJAX。用于在分布式系统中进行通讯。


# 2 环境配置
2.1、安装otp_win32_18.2.1/otp_win64_18.2.1；
这是erlang客户端安装包，RabbitMQ是基于erlang开发；

2.2、安装rabbitmq-server-3.6.1
用于建立RabbitMQ服务
 
2.3、命令行执行语句：pip install pika
安装python调用库


 
# 3 DataService项目应用
3.1 文件mainService\__init__.py
 
MQ_SEND_NAME_LIST：预设发送消息队列名称；
MQ_RECEIVE_NAME：预设接收消息队列名称；
MQ_ADDRESS：RabbitMq服务地址；
MQ_USERNAME：RabbitMq服务用户名；
MQ_PASSWORD：RabbitMq服务密码；

3.2 新增文件夹mod_MsgQueue
 
3.2.1 __init__.py
 
3.2.2 mqServer.py
3.2.2.1 往消息队列中增加消息（生产者）
（1）调用下图接口mqSendTask，
 
（2）该接口会调用内部函数
 
3.2.2.2 从消息队列中取消息（消费者）
（1）创建监控线程
 
（2）监控线程会调用内部函数
 
（3）启动监控线程（详见下方4.3）


3.3 文件BeopService.py
 


 
# 4 相关概念
4.1 消息队列Queue
如果有多个消费者同时订阅同一个Queue中的消息，Queue中的消息会被平摊给多个消费者，而不是每个消费者都收到所有消息。


4.2 Message acknowledgment
消费者处理完业务逻辑后，需发送ack以在MQ中清除该消息；
没有timeout；

4.3 Message durability
如果我们希望即使在RabbitMQ服务重启的情况下，也不会丢失消息，我们可以将Queue与Message都设置为durable=True

4.4 Prefetch count
通过prefetch_count来限制Queue每次发送给每个消费者的消息数，比如我们设置prefetch_count=1，则Queue每次给每个消费者发送一条消息；消费者处理完这条消息后Queue会再给该消费者发送一条消息。


# 5 用户权限设置
设置用户权限的步骤如下：
在安装了mqserver的计算机上，使用命令行来完成以下的步骤，先进入目录：C:\Program Files\RabbitMQ Server\rabbitmq_server-3.6.1\sbin，该目录下有个批处理文件，rabbitmqctl.bat，消息队列的管理和配置全部是由它来完成的。
1）	先删除掉系统默认的用户
输入“rabbitmqctl delete_user guest”，删除掉系统默认的“guest”用户，因为默认安装好rabbitServer之后，guest是administrator权限，没有访问的限制。
2）	增加自定义的用户
输入“rabbitmqctl add_user MyUserName MyPassword”，这句话的含义是添加“MyUserName”这个用户，并设置密码是“MyPassword”
3）	设置用户的标签
输入“rabbitmqctl set_user_tags MyUserName administrator”，设置用户的标签。
4）	添加用户的访问权限
输入“rabbitmqctl set_permissions -p / MyUserName  ".*" ".*" ".*"”，意思是设置用户“MyUserName ”具有全部的访问权限，“/”的含义是visual host，我们现在还用不着，就是一个虚拟的名称，可以指定用户在这个名称下有效，并具有一定的权限。
5）启动用户的授权
输入“rabbitmqctl authenticate_user MyUserName MyPassword”。这样，客户端需要访问mq，就需要用户名和密码了
