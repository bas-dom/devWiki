

1.下载tar包
Wget https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2

2.解压tar包
tar jxvf phantomjs-2.1.1-linux-x86_64.tar.bz2


3.复制解压后的文件夹到/usr/local/src/phantomjs目录
mv phantomjs-2.1.1-linux-x86_64 /usr/local/src/phantomjs

4.生成快捷命令
ln -sf /usr/local/src/phantomjs/bin/phantomjs /usr/local/bin/phantomjs

5.终端输入
Phantomjs –version查看是否安装成功


参考资料：http://www.cnblogs.com/LH2014/p/4073881.html
