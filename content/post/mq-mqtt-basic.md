---
title: "MQTT And Kafka"
date: 2022-06-29T16:52:42+08:00
categories:
- MQ
- MQTT
tags:
- MQTT
keywords:
- mqtt
- kafka
clearReading: true
comments: false
showTags: true
showPagination: true
showSocial: true
showDate: true
---

Some relationship between MQTT and Kafka.

<!--more-->

{{< toc >}}

# MQTT VS Kafka



The main motive behind Kafka is scalability.

MQTT is a protocol with public specification for lightweight 
client / message broker communications, allowing publish/subscribe 
exchanges. Multiple implementations of client libraries and 
brokers (Mosquitto, JoramMQ...) exist and are virtually compatible. 
MQTT just specifies the transport, and vaguely the application part (i.e. how data is handled and possibly stored, how clients are authorized...). The spec is not clear if data consumed on a topic is only real-time or possibly persistent. The spec doesn't state anything about how the message broker implementing MQTT could/should scale.

On the other hand, Apache Kafka is a message broker based on an internal "commit log": its focus is storing massive amounts of data on disk, and allowing consumption in real-time or later (as long as data is still available on disk). It's designed to be deployable as cluster of multiple nodes, with good scalability properties. Kafka uses its own network protocol.

So you are comparing two different things here: a standard pub/sub protocol (with multiple implementations), and a specific message storing/distributing software, vaguley of the same family with its own protocol.

I'd say that if you need to store massive amount of messages, to ensure batch processing, look more at Kafka. If you have lots of clients/apps exchanging messages in real-time on many independent topics look more at the MQTT (or even AMQP) message broker implementations.





![另外一篇关于整体解决方案的图片](https://www.kai-waehner.de/wp-content/uploads/2021/02/Apache-Kafka-and-MQTT-for-Real-Time-Analytics-and-Machine-Learning-with-TensorFlow.jpg)


