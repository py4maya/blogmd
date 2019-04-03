---
title: 一个空的kafka 接收数据 
date: 2019-03-29 13:40:43
tags: [python,kafka]
categories: 开发
---

## 简单代码实现如下:

```python
from kafka import KafkaConsumer

consumer = KafkaConsumer(
        'topic-name',
        bootstrap_servers='.....com:9092',
        enable_auto_commit=False,
        api_version=(0, 8, 2, 1)
)

for msg in consumer:
    print(msg)

```

## 运行监控:

```shell
$ ./tools/kafka_2.10-0.9.0.1/bin/kafka-consumer-offset-checker.sh --group group_name --zookeeper=zookeeper_server --topic yourtopic
```

## 网卡流量监控:

```shell
$ sar -n DEV 10
```

