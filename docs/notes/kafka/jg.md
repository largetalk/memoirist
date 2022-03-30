# Kafka 源码

### Kafka Server包含如下组件：

##### KafkaScheduler

就是把java.util.concurrent.ScheduledThreadPoolExecutor 包装一下

##### ZkClient

zookeeper client包装

##### LogManager

管理该broker上的topic-partition的Log，index文件

##### SocketServer

kafka reactor反应器，处理kafka Apis

##### ReplicaManager

##### KafkaController

##### Apis 和 RequestHandlerPool

定义了kafka api和处理的线程池
处理了

```
ProduceRequest
FetchRequest
OffsetRequest
LeaderAndIsrRequest
StopReplicaRequest
UpdatemetaRequest
ControlledShutdownRequest
OffsetCommitRequest
OffsetFetchRequest
```

##### Mx4jLoder

提供jmx程序监控功能

##### TopicConfigManager

订阅zookeeper上topic change的通知

##### KafkaHealthCheck

到zookeeper注册broker路径

### Kafka ZkUtil

```scala
  val ConsumersPath = "/consumers"
  val BrokerIdsPath = "/brokers/ids"
  val BrokerTopicsPath = "/brokers/topics"
  val TopicConfigPath = "/config/topics"
  val TopicConfigChangesPath = "/config/changes"
  val ControllerPath = "/controller"
  val ControllerEpochPath = "/controller_epoch"
  val ReassignPartitionsPath = "/admin/reassign_partitions"
  val DeleteTopicsPath = "/admin/delete_topics"
  val PreferredReplicaLeaderElectionPath = "/admin/preferred_replica_election"

  def getTopicPath(topic: String): String = {
    BrokerTopicsPath + "/" + topic
  }

  def getTopicPartitionsPath(topic: String): String = {
    getTopicPath(topic) + "/partitions"
  }

  def getTopicConfigPath(topic: String): String = 
    TopicConfigPath + "/" + topic

  def getDeleteTopicPath(topic: String): String =
    DeleteTopicsPath + "/" + topic

  def getTopicPartitionPath(topic: String, partitionId: Int): String =
    getTopicPartitionsPath(topic) + "/" + partitionId

  def getTopicPartitionLeaderAndIsrPath(topic: String, partitionId: Int): String =
    getTopicPartitionPath(topic, partitionId) + "/" + "state"


class ZKConfig(props: VerifiableProperties) {
  /** ZK host string */
  val zkConnect = props.getString("zookeeper.connect")

  /** zookeeper session timeout */
  val zkSessionTimeoutMs = props.getInt("zookeeper.session.timeout.ms", 6000)

  /** the max time that the client waits to establish a connection to zookeeper */
  val zkConnectionTimeoutMs = props.getInt("zookeeper.connection.timeout.ms",zkSessionTimeoutMs)

  /** how far a ZK follower can be behind a ZK leader */
  val zkSyncTimeMs = props.getInt("zookeeper.sync.time.ms", 2000)
```