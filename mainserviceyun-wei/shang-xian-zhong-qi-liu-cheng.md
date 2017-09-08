先cd 进入mainService目录，更新代码

```
#svn up
```

生产环境使用gunicorn，重启方法使用：

```
kill -HUP PPID号码
```

如果出现启动后总是退出，那么就用手动运行python命令行验证：

```
python runEngine.py
```

然后观察报什么错误，根据错误进行修复。

最常见的错误就是开发用到了新的第三方库，服务器上线前需要pip安装一次。

