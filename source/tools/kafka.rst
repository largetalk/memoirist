Kfaka
=====================

Kafka™ is used for building real-time data pipelines and streaming apps

概念
---------------

producer: 发送数据到topic。producer的指定一条记录发送到topic的哪个partition，可以简单的轮询，也可以根据一些语义区分（根据记录中的key）

consumer: consumer给自己标上一个consumer group name, 发布到topic的每条记录会被交付给每个consumer group中的一个consumer实例。


