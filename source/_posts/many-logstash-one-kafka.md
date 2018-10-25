---
title: 多logstash 消费同一个kafka 做队列数据转发 
date: 2018-10-25 13:02:20
tags: [logstash,kafka]
categories:
---


logstash-2.4.0 

### 修改如下的代码: 

```
vendor/bundle/jruby/1.9/gems/jruby-kafka-1.5.0-java/lib/jruby-kafka/group.rb

run 部分的代码:  大约163行
if @reset_beginning == 'from-beginning'
    #Java::kafka::utils::ZkUtils.maybeDeletePath(@zk_connect, "/consumers/#{@group_id}")
end

```

### 批量创建配置文件的脚本:

```
#!/bin/bash

rm -f conf/*
num=10

for((in=0;in<$num;in++))
do
    cat hflow.conf  | sed "s/<data_num>/${in}/g" > conf/hflow_${in}.conf
done


```

### hflow.conf 的模板:
```
input {
    kafka {
        zk_connect => "zk.your.own.com:2181/kafka/k1001"
        topic_id => ""
        group_id => ""
        reset_beginning => true
        consumer_threads => 1
        decorate_events => false 
    }
}
    
output {

    file{
        path => "/data0/logstash/logstash-2.4.0/data/hflow_%{+YYYY-MM-dd}_<data_num>.log"    
    }

    file{
        path => "/data0/logstash/logstash-2.4.0/data/all_%{+YYYY-MM-dd}.log"    
    }
}
```

### supervisor 配置:
```
[program:starflow]
process_name=%(program_name)s_%(process_num)01d;
numprocs=10
command=/data0/logstash/logstash-2.4.0/bin/logstash -f /data0/logstash/logstash-2.4.0/conf/hflow_%(process_num)01d.conf
directory=/data0/logstash/logstash-2.4.0
autostart=true
startsecs=1
startretries=5
user=root
redirect_stderr=true
stdout_logfile=/data0/logstash/logstash-2.4.0/logs/stdout.log
stdout_logfile_maxbytes=1024MB
stdout_logfile_backups=10
```
