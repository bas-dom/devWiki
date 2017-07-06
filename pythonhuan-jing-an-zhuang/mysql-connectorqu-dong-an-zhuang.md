# MySQL python驱动版本说明 #

mysql-connector-python驱动目前官方网站版本为2.1以上

由于该驱动与老版本不兼容（新版本将set_database方法删除了），本系统代码适配版本为1.2.2，因此安装时的命令为：

```

pip3 install --egg http://dev.mysql.com/get/Downloads/Connector-Python/mysql-connector-python-1.2.2.zip

```

如果误安装为: 2.1.4，将会导致mysql数据库无法访问.


