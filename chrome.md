
# Chrome默认不能访问21端口的http服务器？#

需要做设置才能访问

chrome错误提示代码：ERR_UNSAFE_PORT
原因：chrome有一个默认非安全端口列表，21正好包含在里面了
解决方法：
    选中Google Chrome 快捷方式，右键属性，在”目标”对应文本框添加：
    --explicitly-allowed-ports=端口1，端口2。。。以此类推
可参考：

```
http://blog.csdn.net/testcs_dn/article/details/39186225 
```