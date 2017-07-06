ECS最新上线后都会屏蔽掉25端口，因此如果需要一台线上服务器通过smtp发送邮件的话，需要规避25端口发送，而使用465，并使用SSL进行邮件发送。

以下为阿里企业邮箱验证后可以使用的配置(Flask-mail配置)


```
    MAIL_SERVER='smtp.*******.com',
    MAIL_PORT = 465,
    MAIL_USE_TLS = False,
    MAIL_USE_SSL = True,
    MAIL_USERNAME='**********',
    MAIL_PASSWORD='***********',
    MAIL_DEFAULT_SENDER=("*****", "*******"),
    MAIL_DEBUG=False,
    MAIL_SUPPRESS_SEND = False
```