# RabbitMq 新增用户与权限配置 #
## 管理员登录 ##

连接地址：http://RABBITMQ_SERVER_IP:15672/

输入管理员帐号和密码

## 新增用户 ##
 

输入用户名与密码，在Tags处，有各权限对应的提示，如要建立只有消息收发权限的用户，填入与提示权限不相同的字符即可，一般原则，与用户名相同字符，点击Add user，生成的该用户没有登录操作界面的权限。

## 用户可访问镜像 ##

 
在用户列表中，点击新建的用户，进入镜像访问配置界面。
 

在Virtual Host处选择“/”，使用户有连接根目录的权限，点击“Set permission”。


## 主题与队列 ##
 
点击“Exchanges”标签，进入界面后，继续点击“amq.topic”进入主题与队列绑定界面。
 

在“To queue”栏目中填入需要绑定的队列，在“Routing key”中写入字符串，这一字符串将用于代码识别，点击“Bind”，配置完成。


# 代码示例
## 监听端示例代码

```
import paho.mqtt.client as mqtt
import requests
import json

# The callback for when the client receives a CONNACK response from the server.
def on_connect(client, userdata, flags, rc):
    print("Connected with result code "+str(rc))
    print(flags)

    # Subscribing in on_connect() means that if we lose the connection and
    # reconnect then subscriptions will be renewed.
    #client.subscribe("$SYS/#")
    client.subscribe("请填写Routing key")

# The callback for when a PUBLISH message is received from the server.
def on_message(client, userdata, msg):
    #print("msg.topic:"+msg.topic+"     msg.payload:"+str(msg.payload)+"   msg:"+str(msg))
    encodedjson=msg.payload.decode(encoding='utf-8')
    print(encodedjson)
    dict = json.loads(encodedjson)
    pointNameList=dict['pointName']
    pointValueList=dict['pointValue']
    print(dict)


def set_realtime_data(projId, pointNameList, pointValueList, flag):
    rt = False
    if isinstance(pointNameList, list) and isinstance(pointValueList, list):
        if len(pointNameList) == len(pointValueList):
                data = dict(projId=projId, point=pointNameList, value=pointValueList, flag=flag)
                try:
                    r = requests.post("http://127.0.0.1:5000/set_mutile_realtimedata_by_projid", data=json.dumps(data),headers={'content-type': 'application/json'})
                    if r.status_code == 200:
                        rv = json.loads(r.text)
                        if rv.get('state') == 1:
                            rt = True
                    else:
                        print(r.status_code)
                except Exception as e:
                    print(e)
        return rt

client = mqtt.Client()
client.on_connect = on_connect
client.on_message = on_message
client.username_pw_set("MyUser", "MyPassword")


print("client_id:"+client._client_id);

client.connect("localhost", 1883, 60)

client.loop_forever()






```

## 发送端示例代码

```
import paho.mqtt.client as mqtt
import json

def on_publish(mqttc, obj, mid):    
        print("OnPublish, mid: "+str(mid))

def on_connect(client, userdata, flags, rc):
    print("Connected with result code "+str(rc))
    print(flags)
    #mqttc.subscribe("MQTTDataUpload", 0)

        
if __name__ == '__main__': 
    #mqttc = mqtt.Client(client_id="newClient", clean_session=True, userdata=None)
    mqttc = mqtt.Client()
    
    mqttc.on_connect = on_connect
    mqttc.on_publish = on_publish
   
    strBroker = "RABBIT_MQ_SERVER" 
       
    mqttc.connect(strBroker, 1883, 60)     

    #mqttc.loop_forever()
    #strMessage="YOUR MESSAGE STR"
    """
    publish(topic, payload=None, qos=0, retain=False)
    topic:the topic that the message should be published on
    payload:the actual message to send
    qos:the quality of service level to use
    retain:if set to True, the message will be set as the "last known good"/retained message for the topic.
    """
    jsonMessage={"projectKey":"myproject","pointName":["_pointName_aaaa","_pointName_2bbb"],"pointValue":[1.2,2.3],'t_time':["2017-7-29 10:00:00","2017-7-29 10:00:00"]}
    strMessage=json.dumps(jsonMessage)
    mqttc.publish("请填写Routing key",strMessage, 0, False)    
    mqttc.disconnect()   
    
    
    

```
