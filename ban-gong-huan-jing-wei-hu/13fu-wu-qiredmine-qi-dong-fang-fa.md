服务器地址为公网（79.46）

进入C盘domi的redmine目录运行：

```
set RAILS_ENV=production
rails s -b 0.0.0.0 -p 80
```

第一步一定要做，否则环境变量会认为是development，会有数据库连接错误。



# 附录: redmine安装要点

1. 安装ruby, devKit, rails(用gem insall rails), gem install...mysql
   > devKit安装后的解压路径要放在系统path里，确认make.exe在path里
   > 版本一定要与redmine要求符合，比如redmine3.3.3 要求 ruby:
   
   Redmine version	Supported Ruby versions	Rails version used
3.3	ruby 1.9.34, 2.0.03, 2.1, 2.22, 2.3	Rails 4.2
   
2. 安装svn命令行工具, 下载地址： https://www.visualsvn.com/downloads/，并加入path
3. 配置configure.yml里的scm 命令为svn
4. 按照redmine install教程初始化。启动redmine，确保svn在path里



