<link href="http://kevinburke.bitbucket.org/markdowncss/markdown.css" rel="stylesheet"></link>

###DTU信息更新流程

##将实时数据和历史数据发送到service处理

```
bool CDTUServerHandler::UnZipRealFileAndSendPython( realfilezip zipfile)
```


##将消息按标识分配不同的处理类,正常core连上来每分钟都会触发这个函数
```
bool CDTUServerDlg::SetData( string strDTUName,const char* rawbuffer, const int rawsize ,CTCPCustom* pTcpCustom)
```

##所有的tcp收到的消息入口
```
void CALLBACK CDTUServerDlg::OnClientRead(void* pOwner,CTCPCustom* pTcpCustom,const char * buf,DWORD dwBufLen )   
```

##设置service后台中dtu的信息
具体在service中的响应api : site/v1.0/multiUpdateDTUProjectInfo

```
bool CDTUServerDlg::UpdateDTUInfoByPython()

```