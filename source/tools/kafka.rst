Kfaka
=====================

Kafka™ is used for building real-time data pipelines and streaming apps

概念
---------------

producer: 发送数据到topic。producer的指定一条记录发送到topic的哪个partition，可以简单的轮询，也可以根据一些语义区分（根据记录中的key）

consumer: consumer给自己标上一个consumer group name, 发布到topic的每条记录会被交付给每个consumer group中的一个consumer实例。

topic: kafka集群用来存储消息流的类别名, 每个消息由key，value和timestamp组成

leader: 负责给定partition所有读写操作的节点，每个partition会随机选择出一个leader

replicas: 备份partition日志的节点列表，包括leader

irs: 当前活着并且跟上leader的节点列表, 在leader死掉可以被选举为leader

DESIGN
---------------------

high-throughput,  large data backlogs,  low-latency 

file system, sequence read/write, pagecache, sendfile, end-to-end batch compress

producer: load balancing, asynchronous send

comsumer: pull, record comsumer position at consumer side

at-least one 

故障节点重新加入集群要完全复制数据

Disable unclean leader election -- 数据一致性高于系统可用性

Specify a minimum ISR size -- 降低可用性保证数据安全性

zk directories
-----------------------

/brokers/ids/[0...N] --> {"jmx_port":...,"timestamp":...,"endpoints":[...],"host":...,"version":...,"port":...} (ephemeral node)

/brokers/topics/[topic]/partitions/[0...N]/state --> {"controller_epoch":...,"leader":...,"version":...,"leader_epoch":...,"isr":[...]} 

/consumers/[group_id]/ids/[consumer_id] --> {"version":...,"subscription":{...:...},"pattern":...,"timestamp":...} (ephemeral node)
    
/consumers/[group_id]/offsets/[topic]/[partition_id] --> offset_counter_value (persistent node)

/consumers/[group_id]/owners/[topic]/[partition_id] --> consumer_node_id (ephemeral node)

Consumer
-----------------

When a consumer starts, it does the following:

* Register itself in the consumer id registry under its group.
* Register a watch on changes (new consumers joining or any existing consumers leaving) under the consumer id registry. (Each change triggers rebalancing among all consumers within the group to which the changed consumer belongs.)
* Register a watch on changes (new brokers joining or any existing brokers leaving) under the broker id registry. (Each change triggers rebalancing among all consumers in all consumer groups.)
* If the consumer creates a message stream using a topic filter, it also registers a watch on changes (new topics being added) under the broker topic registry. (Each change will trigger re-evaluation of the available topics to determine which topics are allowed by the topic filter. A new allowed topic will trigger rebalancing among all consumers within the consumer group.)
* Force itself to rebalance within in its consumer group.

