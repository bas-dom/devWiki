# 实时计算点重启方法
  实时计算和诊断服务器上已经使用supervisord进行进程守护
  
  具体supervisord配置参考相关文档。

##　登录到远程计算机
SSH登录到远程Linux服务器上（建议使用XShell客户端）


## 结束实时计算的进程
现在输入：ps –ef，可以看到当前系统的进程信息。
 
运行如下命令停止所有计算和诊断：
```
supervisorctl stop all
```


## 重新启动进程

停止后可以对文件进行覆盖和更新，再运行如下命令重启所有进程：
```
supervisorctl start all
```

也可以利用如下命令重启（先关闭后启动）：

```
supervisorctl reload all
```

