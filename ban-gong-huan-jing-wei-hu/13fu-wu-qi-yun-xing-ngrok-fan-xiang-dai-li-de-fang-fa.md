运行cmd命令：

```
ngrok.exe http 80
```

MAC上运行时需要用 attr -d 去掉com....那个文件属性

同时 chmod 777 ./ngrok

本机开node的server构造函数中 ，如下选项要加上，否则会显示 Invalid Host Header.

```
disableHostCheck: true
```



