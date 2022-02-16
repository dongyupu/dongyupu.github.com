---
layout: post
title: "kubernetes log solustion:Log-pilot + Kafka + Logstash + Elasticsearch"
subtitle:   "Log-pilot + Kafka + Logstash + Elasticsearch"
date:       2019-06-27 14:16:22 +0800
author:     "Dongyupu"
header-img: "img/bg-starfield.gif"
---
## kubernetes log solustion:Log-pilot + Kafka + Logstash + Elasticsearch

- Log-pilot:Collect logs for docker containers
  - https://github.com/AliyunContainerService/log-pilot
  - 遇到的问题：https://github.com/AliyunContainerService/log-pilot/issues/117
    - 报错：Field [_timestamp] is a metadata field and cannot be added inside a document.
    - 修改 cfg.FormatConfig["time_key"] = "_timestamp" 去掉下划线后重新编译就可以了
    - es7.1.0 , log-pilot 0.9.7-fluentd
- Kafka
  - 使用 https://github.com/Yolean/kubernetes-kafka 的chart部署的
- Logstash
  - 使用stable的chart部署的 https://github.com/helm/charts/tree/master/stable
  - 配置如下

  ~~~yml
  inputs:
    main: |-
      input {
        #kafka输入源配置
        kafka {
          ## ref: https://www.elastic.co/guide/en/logstash/current/plugins-inputs-kafka.html
          #kafka集群地址
          bootstrap_servers => "kafka.elasticsearch:9092"
          #这是kafka中的消费组者ID，默认值是“logstash”。kafka将消息发到每个消费者组中，同一个组中的消费者收到的数据不重复。例如有两个消费者组G1、G2，G1中有成员A、B，G2中有成员C、D。kafka从输入中收到了10条消息，会将这10条消息同时发送给G1和G2，A和B各会收到这10条消息中的一部分，他们收到消息的并集就是这10条消息，C和D同理。
          group_id => "logstash"
          codec => json { charset => "UTF-8" }
          #logstash的消费线程，一般一个线程对应kafka中的一个partition（分区），同一组logstash的consumer_threads之和应该不大于一个topic的partition，超过了就是资源的浪费，一般的建议是相等。
          consumer_threads => 1
          #从kafka中哪个topic读取数据，这里的topic名要与filebeat中使用的topic保持一致
          #topics => ["gitlab-ce"]
          #使用正则表达式适配所有的topic
          topics_pattern  => "^[a-zA-Z].*"
          type => "logs"
         }
      }
  
  outputs:
    main: |-
      output {
        elasticsearch {
          hosts => ["${ELASTICSEARCH_HOST}:${ELASTICSEARCH_PORT}"]
          user => elastic
          password => password
          manage_template => false
          #index => "%{[@metadata][beat]}-%{+YYYY.MM.dd}"
          #直接引用参数%{topic}，这种格式%{[@metadata][topic]}无法获取到想要的参数
          index => "%{topic}-%{+YYYY-MM-dd}"
        }
      }
  ~~~
   
- Elastic search
  - 新版本的es已经将安全功能xpack免费开源了，不必再去破解xpack了，我安装的版本是7.1.0的镜像
  - 使用 https://github.com/helm/charts/tree/master/stable/elasticsearch 安装
