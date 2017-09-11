# 问题 #
在前端js语言中，使用ajax的post或者get去请求后台的静态文件，不能触发浏览器的下载文件事件，只能变成字节流传输，怎样才能实现浏览器提示下载或者chrome直接下载为默认目录中的文件？

但浏览器地址栏里直接输入get的 url是能够触发下载的。

# 分析 #
ajax的请求均为**异步**请求，而浏览器地址栏的地址输入后发出的是**同步**请求

因此只能特殊处理。

# 解决方案 #
## 方案1 ##
服务端存成临时文件，ajax 返回地址，然后 js 拿到地址，动态创建下载链接模拟用户点击下载，大概如下：
![](http://ww1.sinaimg.cn/large/006qm7Cpgy1fjfo5ylh7mj30c40773ye.jpg)

## 方案2 ##

服务端返回字节流，客户端自行解码下载（不能用 jQuery，需要使用原生的 XMLHttpRequest），大概如下：
![](http://ww1.sinaimg.cn/large/006qm7Cpgy1fjfo5h8ep8j30k40andg0.jpg)

## 方案选择 ##
两种方案任选其一。
如果文件在服务端是以文件方式已经存储好的，建议第一种；
使用三方控件，动态生成的，使用第二种


参考链接：
[https://stackoverflow.com/questions/34586671/download-pdf-file-using-jquery-ajax](https://stackoverflow.com/questions/34586671/download-pdf-file-using-jquery-ajax)