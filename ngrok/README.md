1 首先下载ngrok，解压到相对应的目录。下载地址

[https://ngrok.com/download](https://ngrok.com/download)

2 启动项目的本地服务（我用的是node）

3 打开ngrok解压的目录下，创建一个ngrok.cfg文件，并填写内容如下：

server\_addr: "tunnel.echomod.cn:4443"

trust\_host\_root\_certs: false

  


4 在webpack的devServer配置中添加 disableHostCheck: true.

webpack-dev-server修改了一些东西，默认检查hostname。如果hostname不是配置内的，将不可访问。



5 到ngrok的官网验证，得到authtoken ,并使用ngrok authtoken xxxxx 验证

6 使用ngrok http xxx \(xxx是你本地服务器开的端口\)

