# python+selenium构建自动化测试环境
### 一、前期准备
* 通过`https://www.python.org/ftp/python/3.4.4/python-3.4.4-macosx10.6.pkg
`链接安装python3.4.4（默认会安装上pip3）
* pycharm【python开发环境】
* firefox或者Chorme浏览器

### 二、安装步骤
1. 在根目录下，执行sudo pip install -U -selenium
2. 新建一个test.py ，输入

            from selenium import webdriver
            import time
            dr = webdriver.Firefox()
            time.sleep(5)
            print 'Browser will be closed'
            dr.quit()
            print 'Browser is close'
3. 在目录下运行python test.py报错
    * 如果是火狐:

            Traceback (most recent call last):
          File "test.py", line 5, in <module>
            dr = webdriver.Firefox()
          File "/usr/local/lib/python2.7/site-packages/selenium/webdriver/firefox/webdriver.py", line 142, in __init__
            self.service.start()
          File "/usr/local/lib/python2.7/site-packages/selenium/webdriver/common/service.py", line 81, in start
            os.path.basename(self.path), self.start_error_message)
            selenium.common.exceptions.WebDriverException: Message: 'geckodriver' executable needs to be in PATH. 
    
    * Chrome下报错
    
            Traceback (most recent call last):
              File "test.py", line 5, in <module>
                dr = webdriver.Chrome()
              File "/usr/local/lib/python2.7/site-packages/selenium/webdriver/chrome/webdriver.py", line 62, in __init__
                self.service.start()
              File "/usr/local/lib/python2.7/site-packages/selenium/webdriver/common/service.py", line 81, in start
                os.path.basename(self.path), self.start_error_message)
            selenium.common.exceptions.WebDriverException: Message: 'chromedriver' executable needs to be in PATH. Please see https://sites.google.com/a/chromium.org/chromedriver/home

4. 报错原因是因为在环境变量中缺少可执行的文件，根据自己的需求安装下面两个包解决错误（默认安装在/usr/local/bin 目录下）：

        brew instatll geckodriver (FireFox)
        brew install chromedriver(Chorme)
然后再在项目的目录下执行 **python test.py** ，当firefox浏览器打开然后再过一点时间关闭并且打印出
        
    Browser will be closed
    Browser is close

则表示成功

5. 然后根据自己的自动化测试项目使用pip3安装对应的包。
