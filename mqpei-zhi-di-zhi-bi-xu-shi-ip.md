

注意：正确的配置写法是：



`MQ_ADDRESS = '139.224.46.62'`



一次升级时将这个地址写为了：

`MQ_ADDRESS = 'http://139.224.46.62'`



导致实时数据永远不更新（接受数据刷入rtadta缓存表的机制触发不了消息）

