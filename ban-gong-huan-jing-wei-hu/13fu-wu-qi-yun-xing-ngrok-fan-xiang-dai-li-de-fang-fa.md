运行cmd命令：

```
ngrok.exe http 80
```

将ngrok拷贝到MAC上运行时可能遇到终端访问文件出现“Permission Denied”的问题，解决方案：

```
一个文件有3种权限，读、写、可执行，你这个文件没有可执行权限，需要加上可执行权限。

1. 终端下先 cd到该文件的目录下

2. 执行命令 chmod a+x ./文件名

这样就可以打开该文件了

或者直接用：chmod 777 ./ngrok
```

\#ls -l

查看文件的属性中如果有@结尾，意味着权限仍然不对，需要用 attr -d 去掉com....那个文件属性

然后执行命令行即可。



本机开node的server构造函数中 ，如下选项要加上，否则会显示 Invalid Host Header.

```
disableHostCheck: true
```



