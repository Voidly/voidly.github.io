---
layout: post
title: "kafka学习笔记"
description: 
modified: 2016-03-05
category: hadoop
tags: [kafka,hadoop]
comments: true
mathjax: 
---

## 背景介绍

Kafka最初由LinkedIn公司基于独特的设计实现为一个分布式的提交日志系统( a distributed commit log)，之后成为Apache项目的一部分。Kafka系统快速、可扩展并且可持久化。它的分区特性，可复制和可容错都是其不错的特性。

## 为何使用消息系统？

#### 解耦
在项目启动之初来预测将来项目会碰到什么需求，是极其困难的。消息队列在处理过程中间插入了一个隐含的、基于数据的接口层，两边的处理过程都要实现这一接口。这允许你独立的扩展或修改两边的处理过程，只要确保它们遵守同样的接口约束。

#### 冗余

有时在处理数据的时候处理过程会失败。除非数据被持久化，否则将永远丢失。消息队列把数据进行持久化直到它们已经被完全处理，通过这一方式规避了数据丢失风险。在被许多消息队列所采用的"插入-获取-删除"范式中，在把一个消息从队列中删除之前，需要你的处理过程明确的指出该消息已经被处理完毕，确保你的数据被安全的保存直到你使用完毕。

#### 扩展性
因为消息队列解耦了你的处理过程，所以增大消息入队和处理的频率是很容易的；只要另外增加处理过程即可。不需要改变代码、不需要调节参数。扩展就像调大电力按钮一样简单。

#### 灵活性 & 峰值处理能力
当你的应用上了Hacker News的首页，你将发现访问流量攀升到一个不同寻常的水平。在访问量剧增的情况下，你的应用仍然需要继续发挥作用，但是这样的突发流量并不常见；如果为以能处理这类峰值访问为标准来投入资源随时待命无疑是巨大的浪费。使用消息队列能够使关键组件顶住增长的访问压力，而不是因为超出负荷的请求而完全崩溃。

#### 可恢复性
当体系的一部分组件失效，不会影响到整个系统。消息队列降低了进程间的耦合度，所以即使一个处理消息的进程挂掉，加入队列中的消息仍然可以在系统恢复后被处理。而这种允许重试或者延后处理请求的能力通常是造就一个略感不便的用户和一个沮丧透顶的用户之间的区别。

#### 送达保证
消息队列提供的冗余机制保证了消息能被实际的处理，只要一个进程读取了该队列即可。在此基础上，IronMQ提供了一个"只送达一次"保证。无论有多少进程在从队列中领取数据，每一个消息只能被处理一次。这之所以成为可能，是因为获取一个消息只是"预定"了这个消息，暂时把它移出了队列。除非客户端明确的表示已经处理完了这个消息，否则这个消息会被放回队列中去，在一段可配置的时间之后可再次被处理。

#### 排序保证
在许多情况下，数据处理的顺序都很重要。消息队列本来就是排序的，并且能保证数据会按照特定的顺序来处理。IronMO保证消息浆糊通过FIFO（先进先出）的顺序来处理，因此消息在队列中的位置就是从队列中检索他们的位置。

#### 缓冲
在任何重要的系统中，都会有需要不同的处理时间的元素。例如,加载一张图片比应用过滤器花费更少的时间。消息队列通过一个缓冲层来帮助任务最高效率的执行--写入队列的处理会尽可能的快速，而不受从队列读的预备处理的约束。该缓冲有助于控制和优化数据流经过系统的速度。

#### 理解数据流
在一个分布式系统里，要得到一个关于用户操作会用多长时间及其原因的总体印象，是个巨大的挑战。消息系列通过消息被处理的频率，来方便的辅助确定那些表现不佳的处理过程或领域，这些地方的数据流都不够优化。

#### 异步通信
很多时候，你不想也不需要立即处理消息。消息队列提供了异步处理机制，允许你把一个消息放入队列，但并不立即处理它。你想向队列中放入多少消息就放多少，然后在你乐意的时候再去处理它们。

## Kafka简介

* 它被设计为一个分布式系统，易于向外扩展；
* 它同时为发布和订阅提供高吞吐量；
* 它支持多订阅者，当失败时能自动平衡消费者；
* 它将消息持久化到磁盘，因此可用于批量消费，例如ETL，以及实时应用程序。

## 架构

它的架构包括以下组件：

* **话题（Topic）** 是特定类型的消息流。消息是字节的有效负载（Payload），话题是消息的分类名或种子（Feed）名。
* **生产者（Producer）** 是能够发布消息到话题的任何对象。
* **代理(Broker)** 已发布的消息保存在一组服务器中，它们被称为代理（Broker）或Kafka集群。
* **消费者** 可以订阅一个或多个话题，并从Broker拉数据，从而消费这些已发布的消息。

架构图如下：

![](https://github.com/Voidly/Img/blob/master/blog/kafka.png?raw=true)


## 常用消息队列对比

||ActiveMQ|RabbitMQ|Kafka|
|:---:|:---:|:---:|:---:|
|所属公司/社区|Apache|Mozillia Public License|Apache/LinkedIn|
|开发语言|Java|Erlang|Java|
|支持的协议|OpenWire、STOMP、REST、XMPP、AMQP|AMQP|仿造AMQP自己定义的一套|
|事务|支持|不支持|不支持|
|集群|支持|支持|支持|
|负载均衡|支持|支持|支持|
|动态扩容|不支持|不支持|支持(zk)|


## AMQP基本概念

#### AMQP处理流程

![](https://github.com/Voidly/Img/blob/master/blog/AMQP.jpg?raw=true)

#### 基本概念

* **消费者(Consumer)** : 从消息队列请求消息的客户端应用程序
* **生产者** : 向broker发布消息的客户端应用程序
* **AMQP服务器端(broker)** : 用来接收生产者发送的消息并将这些消息路由给服务器中的队列

## kafka缺陷

* 一个分区只能用单个盘
* 均衡日志数据是粒度只能到broker
* 由于设计轻，因此broker不保存消费者的状态，需要借助zookeeper保存


## 参考

> *  http://kafka.apache.org/documentation.html
> *  http://www.infoq.com/cn/articles/apache-kafka/
> * https://www.iron.io/top-10-uses-for-message-queue/
