# Ubuntu版本说明

示例所用版本为：14.04

## 1.简单操作命令

ls

ps 

grep

mkdir

vim

## 2.安装Python扩展包

2.1.安装setuptools  
安装Python2.7的setuptools：

sudo apt-get install python-setuptools python-dev build-essential

安装Python3.4的setuptools：

sudo apt-get install python3-setuptools python-dev build-essential

2.2.已安装2.7和3.4并存的系统中切换指向

2.3.安装pip  
安装Python2.7中的pip：

sudoeasy\_install pip

安装Python3.4中的pip：

sudo easy\_install3 pip

2.4.安装Python扩展包  
在Python3.4中安装扩展包，需要使用上一步中安装在Python3.4中的pip工具，即pip3：

pip3 install flask

pip3 install Flask-mail

pip3 install xlrd

pip3 install pymongo

pip3 install configobj

pip3 install flask-compress

pip3 install pdfkit

pip3 install flask-assets

安装mysql-connector-python:

pip3 install mysql-connector-python --allow-external mysql-connector-python

安装numpy：

首先使用apt-get命令安装所有编译所需的库：输入sudo apt-get build-dep python-numpy

再通过pip3安装，输入pip3 install numpy

如果提示所要安装的扩展包已经存在，则使用--upgrade参数更新。如：

pip3install requests --upgrade

pip3install blinker --upgrade

pip3install jinja2 --upgrade

pip3install six --upgrade

## 3.安装nginx 

sudo apt-get install nginx

nginx命令：

servicenginx start

servicenginx restart

servicenginx stop

或者指定nginx所在目录：

sudo /etc/init.d/nginx start

sudo /etc/init.d/nginxstop

# 4.安装SVN

sudo apt-get install RapidSVN

## 5.启动runserver.py 

使用SVN迁出程序代码，使用终端进入到代码所在文件夹目录后输入python runserver.py回车。在浏览器地址栏中输入IP地址即可访问BEOP网站。

# 6.安装和配置vsftps 

6.1安装vsftps  


打开命令行终端，获取root权限，输入sudo apt-get install vsftpd



6.2.启动、停止vsftpd  


启动：servicevsftpd start

重启：servicevsftpd restart

停止：servicevsftpd stop

6.3.创建ftp用户和目录  
新建用户：

sudouseradd –d/home/myFtpRoot –s /bin/bash user1

注：‘/home/myFtpRoot’为FTP根目录，‘user1’为FTP用户名

设置密码：

Sudopasswd mypwd1

修改文件的权限：

Sudochmod 777 /home/myFtpRoot

6.4.配置vsftp.conf文件  
在/etc/vsftpd.conf中添加：

userlist\_deny=NO

userlist\_enable=YES

userlist\_file=/etc/allowed\_users

seccomp\_sandbox=NO

listen\_port=6621

修改：

write\_enable=YES

6.5.配置allowed\_users  
打开命令行终端，输入：sudogedit /etc/allowed\_users

在打开的文件中输入FTP用户名，保存并关闭

## 7.安装wkhtmltopdf 

下载：

wget[http://download.gna.org/wkhtmltopdf/0.12/0.12.2.1/wkhtmltox-0.12.2.1\_linux-trusty-amd64.deb](http://download.gna.org/wkhtmltopdf/0.12/0.12.2.1/wkhtmltox-0.12.2.1_linux-trusty-amd64.deb)

安装：

sudodpkg –i wkhtmltox-0.12.2.1\_linux-trusty-amd64.deb

安装可能会出现错误，提示wkhtmltox依赖于xfonts-75dpi；然而：未安装软件包xfonts-75dpi。所以需要安装xfonts-75dpi：

sudo apt-get install xfonts-75dpi

安装完成后再次安装wkhtmlropdf:

sudodpkg –i wkhtmltox-0.12.2.1\_linux-trusty-amd64.deb

安装成功后，在/usr/local/bin目录下可以见到wkhtmltoimage和wkhtmltopdf。

输入: wkhtmltopdfwww.baidu.com ~/baidu.pdf如果显示成功则安装成功



## 8.安装中文字体 

查看字体

fc-list

查看中文字体

fc-list :lang=zh

安装字体:

mkdir -p /usr/share/fonts/my\_fonts

并从另一台已安装simsun.ttf字体的系统复制这个文件到这个目录。

生成字体索引信息

cd /usr/share/fonts/my\_fonts

mkfontscale

mkfontdir

再次查看已安装的字体

fc-list :lang=zh

