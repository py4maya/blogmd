---
title: python 使用rabbitmq的简单代码
date: 2018-01-04 10:17:51
tags: [rabbitmq,python]
categories: 开发
---

* 安装pika处理rabbitmq协议

```shell
pip install pika
```

* 发送数据到rabbitmq
```python
import pika                                                                                                                                                                
                                                                                                                                                                           
credentials = pika.PlainCredentials('','')                                                                                                                    
connection = pika.BlockingConnection(                                                                                                                                      
        pika.ConnectionParameters('',5672,'/',credentials)                                                                                                       
)                                                                                                                                                                          
channel = connection.channel()                                                                                                                                             
channel.queue_declare(queue='hello')                                                                                                                                       
#发送数据到exchange,exchange保存为空，将发送数据到声明的queue
channel.basic_publish(                                                                                                                                                     
		exchange='jira_publish',                                                                                                                                   
        routing_key='ajira',                                                                                                                                               
        body='Hello World!'                                                                                                                                                
)                                                                                                                                                                          
print " [x] Sent 'Hello World!'"                                                                                                                                           
connection.close() 
```

* 接收数据:
```python
import pika                                                                                                                                                                
                                                                                                                                                                           
def callback(ch, method, properties, body):                                                                                                                                
        print body                                                                                                                                                         
                                                                                                                                                                           
credentials = pika.PlainCredentials('','')                                                                                                                    
connection = pika.BlockingConnection(                                                                                                                                      
        pika.ConnectionParameters('',5672,'/',credentials)                                                                                                       
)                                                                                                                                                                          
channel = connection.channel()                                                                                                                                             
channel.basic_consume(callback,queue='hello',no_ack=True)                                                                                                                  
channel.start_consuming() 
```

**如此就完成了一个基本的消息异步处理流程**
