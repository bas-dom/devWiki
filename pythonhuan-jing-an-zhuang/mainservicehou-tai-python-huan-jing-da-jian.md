# Web_Environment

### 服务器python环境安装过程

1. 确保服务器连接

2. `apt-get install python3.4` 安装python3.4

3. `sudo apt-get install openssh-server `

4. `sudo apt-get install python-setuptools`

5. `sudo easy_install virtualenv` （代替方案: `sudo apt-get install python-virtualenv`）

6. `apt-get install python3-pip` 安装pip3

7. `pip3 install flask` 安装flask框架

8. `pip3 install numpy`

9. `sudo easy_install flask-mail`

10. `pip3 install requests` 

11. `sudo apt-get install sqlite3`

12. `pip3 install flask-socketio`

13. `pip3 install redis`

14. `pip3 install flask-assets `

15. `pip3 isntall pdfkit`

16. `pip3 install configobj`

### 安装pillow

1. `apt-get install libtiff5-dev libjpeg8-dev zlib1g-dev   libfreetype6-dev liblcms2-dev libwebp-dev tcl8.6-dev tk8.6-dev python-tk`

2. `pip3 install pillow`

3. `pip3 install tablib`

4. `pip3 install xlsxwriter`

5. `apt-get install python3-lxml ` (pip3ֱ install lxml需要编译，很难装上) 

6. `pip3 install python-docx`

7. `pip3 install xlrd`

8. `pip3 install flask-compress`

8. `pip3 install --egg http://dev.mysql.com/get/Downloads/Connector-Python/mysql-connector-python-1.2.2.zip`

9. `pip3 install xlwt `

10. `pip3 install flask-assets`

11. `pip3 install pymongo`

### 安装svn进行代码拉取

1. `sudo apt-get install subversion`

2. 使用 `svn co [svn项目地址]` 拉取项目到服务器。

3. 使用 `python3.4 runFrameworkTest.py`运行项目，如果没报错则成功。 