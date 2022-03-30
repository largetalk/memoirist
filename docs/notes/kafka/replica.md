## ReplicaManager

Replica记录一个broker上partition的LEO和highWaterMark

```scala
class Replica(val brokerId: Int,
              val partition: Partition,
              time: Time = SystemTime,
              initialHighWatermarkValue: Long = 0L,
              val log: Option[Log] = None) extends Logging {
  //only defined in local replica
  private[this] var highWatermarkValue: AtomicLong = new AtomicLong(initialHighWatermarkValue)
  // only used for remote replica; logEndOffsetValue for local replica is kept in log
  private[this] var logEndOffsetValue = new AtomicLong(ReplicaManager.UnknownLogEndOffset)
  private[this] var logEndOffsetUpdateTimeMsValue: AtomicLong = new AtomicLong(time.milliseconds)
  val topic = partition.topic
  val partitionId = partition.partitionId
}
```

ReplicaFetcherThread继承于AbstractFetcherThread，主要是执行FetchRequest请求数据存到本地log，更新hw和leo

ReplicaFetcherManager继承于AbstractFetcherManager，管理FetcherThread

ReplicaManager包含了ReplicaFetcherManger用来执行添加和删除FetcherThread的操作，另外becomeLeaderOrFollower函数来决定replica是leader还是follower，而becomeLeaderOrFollower的参数LeaderAndIsrRequest是由KafkaController发出。

```scala
class ReplicaManager(val config: KafkaConfig, 
                     time: Time, 
                     val zkClient: ZkClient, 
                     scheduler: Scheduler,
                     val logManager: LogManager,
                     val isShuttingDown: AtomicBoolean ) extends Logging with KafkaMetricsGroup {
  /* epoch of the controller that last changed the leader */
  @volatile var controllerEpoch: Int = KafkaController.InitialControllerEpoch - 1
  private val localBrokerId = config.brokerId
  private val allPartitions = new Pool[(String, Int), Partition]
  private var leaderPartitions = new mutable.HashSet[Partition]()
  private val leaderPartitionsLock = new Object
  private val replicaStateChangeLock = new Object
  val replicaFetcherManager = new ReplicaFetcherManager(config, this)
  private val highWatermarkCheckPointThreadStarted = new AtomicBoolean(false)
  val highWatermarkCheckpoints = config.logDirs.map(dir => (dir, new OffsetCheckpoint(new File(dir, ReplicaManager.HighWatermarkFilename)))).toMap
  private var hwThreadInitialized = false
  this.logIdent = "[Replica Manager on Broker " + localBrokerId + "]: "
  val stateChangeLogger = Logger.getLogger(KafkaController.stateChangeLogger)
}
```