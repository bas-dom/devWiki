进入D盘的redmine目录运行：
```
set RAILS_ENV=production
rails s -b 0.0.0.0 -p 80
```

第一步一定要做，否则环境变量会认为是development，会有数据库连接错误。