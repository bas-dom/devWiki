# 安装 Python 3.4.3

1. 下载python3.4 win32\(用win32防止有一些包的版本冲突问题）安装包，安装至C:\Python34
2. 将C:\Python34和C:\Python34\Scripts目录加入系统Path中
3. 重启计算机
   ```
   用命令行运行python后可以看到python的版本信息，确认是32位即可
   ```

# 使用PIP安装其他必须的插件库

## Flask

```
pip install flask
```

## Requests

```
pip install requests
```



## mysql-connector-python

安装mysql-connector-python时，有两种方式，推荐使用第二种：

第一种：下载 mysql-connector-python-1.2.2.tar.gz （公网ftp的setup - programming下）

到本地解压后，在解压后的目录中运行：

```
python setup.py install
```


第二种，不需要手动下载代码包：



```
pip3 install --egg http://dev.mysql.com/get/Downloads/Connector-Python/mysql-connector-python-1.2.2.zip
```



