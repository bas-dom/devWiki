# MySQL python驱动版本说明 #

mysql-connector-python驱动目前官方网站版本为2.1以上

由于该驱动与老版本不兼容（新版本将set_database方法删除了），本系统代码适配版本为1.2.2，因此安装必须手动安装。

如果误安装为: 2.1.4，将会导致mysql数据库无法访问.


# 手动安装方法 #
安装mysql-connector-python时，有两种方式，推荐使用第二种：

第一种：下载 mysql-connector-python-1.2.2.tar.gz （公网ftp的setup - programming下）

到本地解压后，在解压后的目录中运行：

```
python setup.py install
```

第二种方案，不需要手动下载代码包：

```
pip3 install --egg http://dev.mysql.com/get/Downloads/Connector-Python/mysql-connector-python-1.2.2.zip
```



