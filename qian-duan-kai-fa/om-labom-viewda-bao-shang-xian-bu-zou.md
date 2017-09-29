# OM-Lab、OM-View打包上线步骤

* #### 在本地运行`npm run build`,将生成的app.html文件和public文件夹放到新建的文件夹，如omlab-package20170929v1。
* #### 将omlab-package20170929v1压缩，并上传到置业的FTP中，ftp://-----:-----@139.196.7.223/deployment/下对应的omlab和omview文件夹下。
* #### 登录服务器ubuntu@119.29.61.31,密码：------,在/home/mainService/beopWeb/static目录下，下载上一步的压缩包，并解压到对应的omlab和omview文件里，并将app.html改名为index.html
* #### `ps -ef` 查找到父进程，并杀掉，重启`source runserver.sh`



