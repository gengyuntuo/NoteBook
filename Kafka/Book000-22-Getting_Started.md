# Kafka
> 目录：
  * [Kafka简介](#intro)

## <a name="intro"/>Kafka简介
Kafka是一个分布式消息系统，使用Scala开发的。支持C\+\+、Java、Python。

###### 核心概念
* producer ：生产者
* consumer ：消费者
* consumer Group ：消费者组
* broker ： Kafka集群中的节点，起到缓存数据的作用
* topic ： 话题，消息的分类
* partition ：topic的分组，一个topic可以有多个分组
* message : 消息

###### 消息系统
订阅模式（publish\-subscribe）
点对点模式（point\-to\-point）

