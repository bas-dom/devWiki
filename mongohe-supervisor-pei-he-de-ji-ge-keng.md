# 想使用supervisor和mongo配合的几个设置注意
1）supervisord -c /etc/supervisord.conf，注意第一个命令不是supervisorctl

2）mongo.conf或者命令行里fork必须为false（或去掉）

3）supervisor.conf里不能有nohup &，正确配置段是：

 [program:mongoGolding1]
command=mongod --config /mnt/project/db_configure/mongod.conf
autostart = true
autorestart = true
startsecs = 5

[program:mongoGolding2]
command=mongod --config /mnt/project/db_hisdata/mongod.conf
autostart = true
autorestart = true
startsecs = 5
 




# Supervisor监看mongo时的意外#
supervisor监看mongo时出的意外：mongo意外重启后需要很长世间（比如2分钟），结果supervisor就拼命重启，一下子全乱，所以supervisor还是不靠谱的！（是不是有设置参数判断多久算掉线？）

[root@iZ232y638ghZ db_configure]# mongod --config /mnt/project/db_hisdata/mongod.conf
about to fork child process, waiting until server is ready for connections.
forked process: 1078
```
注意，这里child process启动不了的话，需要查看mongo的log文件进行原因排查
```

（这里等待了三分钟，而且此时如果用supervisor在重启的话另一个shell里很多卡死现象！不得不重启服务器）

child process started successfully, parent exiting




