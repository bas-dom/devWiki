
# 问题描述 #

flask接口响应里第二次调用就会出现这个意外

# 原因分析 #
google资料后感觉是Thread的问题，用flask的Threaded = False 就不会出现这个问题了。

但是flask一旦这样设置，系统只能响应一个请求，也不可行。

# 解决方案 #
在route响应的处理中单独起一个进程来处理报表请求。处理完返回。