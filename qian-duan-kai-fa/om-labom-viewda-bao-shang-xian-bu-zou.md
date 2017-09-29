# OM-Lab打包上线步骤

在本地运行命令构建发布包：

```
npm run build
```

将生成的app.html文件和public文件夹放到新建的文件夹，如omlab-package20170929v1。

将omlab-package20170929v1压缩，并上传到公网的FTP中，例如：

> ftp://-----:-----@139.196.7.223/deployment/下对应的omlab和omview文件夹下。

SSH登录服务器ubuntu@119.29.61.31,密码：------, 在/home/mainService/beopWeb/static目录下，下载上一步的压缩包，并解压到对应的omlab和omview文件里，并将app.html改名为index.html



# OM-View打包上线步骤

与omlab类似，只是压缩包解压到static/omview中



# 注意：更新前端文件不需要重启后台

如果万不得已，需要重启后台，重启方法：

`ps -ef` 找到gunicorn的PPID，然后使用重启命令：

```
kill -NOHUP PPID
```



