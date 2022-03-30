## LogManager

LogConfig 记录segment大小，滚动时间， flush时间，删除时间等等。

```scala
  val SegmentBytesProp = "segment.bytes"
  val SegmentMsProp = "segment.ms"
  val SegmentIndexBytesProp = "segment.index.bytes"
  val FlushMessagesProp = "flush.messages"
  val FlushMsProp = "flush.ms"
  val RetentionBytesProp = "retention.bytes"
  val RententionMsProp = "retention.ms"
  val MaxMessageBytesProp = "max.message.bytes"
  val IndexIntervalBytesProp = "index.interval.bytes"
  val DeleteRetentionMsProp = "delete.retention.ms"
  val FileDeleteDelayMsProp = "file.delete.delay.ms"
  val MinCleanableDirtyRatioProp = "min.cleanable.dirty.ratio"
  val CleanupPolicyProp = "cleanup.policy"
```

CleanerConfig  //log cleaner 配置

#### LogManager

```scala
class LogManager(val logDirs: Array[File],
                 val topicConfigs: Map[String, LogConfig],
                 val defaultConfig: LogConfig,
                 val cleanerConfig: CleanerConfig,
                 val flushCheckMs: Long,
                 val flushCheckpointMs: Long,
                 val retentionCheckMs: Long,
                 scheduler: Scheduler,
                 private val time: Time) 
```

LogManager 持有众多Log，一个Log对应一个topic-partion，LogManager扫描目录，构建生成Logs，然后启动retention， flusher，checkpoint三个定时任务，其他还包括创建Log，删除Log，清除过期Log，Flush等函数。还有checkpoint的管理。

```scala
class Log(val dir: File,
          @volatile var config: LogConfig,
          @volatile var recoveryPoint: Long = 0L,
          val scheduler: Scheduler,
          time: Time = SystemTime) extends Logging with KafkaMetricsGroup {
}
```

Log表示一个topic-partion以及对它的操作，持有一个LogSegments的list，segments按offset排列，一般只操作最后一个segment。Log从日志目录load 数据，处理生成index文件，swap文件等，一个LogSegment对应一个文件和开始的offset，Log类还包含roll新LogSegment，追加记录等功能。

```scala
@nonthreadsafe
class LogSegment(val log: FileMessageSet, 
                 val index: OffsetIndex, 
                 val baseOffset: Long, 
                 val indexIntervalBytes: Int,
                 time: Time) extends Logging {
}
```

LogSegment表示一个segment文件，包含一个FileMessageSet，也就是log file的FileChannel， 一个OffsetIndex是index文件的MappedByteBuffer, 写入LogSegment时用的是ByteBufferMessageSet，OffsetIndex使用二分查找找到目标offset的位置

```scala
class FileMessageSet private[kafka](@volatile var file: File,
                                    private[log] val channel: FileChannel,
                                    private[log] val start: Int,
                                    private[log] val end: Int,
                                    isSlice: Boolean) extends MessageSet with Logging {
 }
 ```

 FileMessageSet包含一个fileChannel，发送数据时使用了fileChannel.transferTo直接写入到socketChannel，避免一次拷贝

 然后是Message类，定义了一个消息的结构, 平平无奇
 ```scala
object Message {
  
  /**
   * The current offset and size for all the fixed-length fields
   */
  val CrcOffset = 0
  val CrcLength = 4
  val MagicOffset = CrcOffset + CrcLength
  val MagicLength = 1
  val AttributesOffset = MagicOffset + MagicLength
  val AttributesLength = 1
  val KeySizeOffset = AttributesOffset + AttributesLength
  val KeySizeLength = 4
  val KeyOffset = KeySizeOffset + KeySizeLength
  val ValueSizeLength = 4
  
  /** The amount of overhead bytes in a message */
  val MessageOverhead = KeyOffset + ValueSizeLength
  
  /**
   * The minimum valid size for the message header
   */
  val MinHeaderSize = CrcLength + MagicLength + AttributesLength + KeySizeLength + ValueSizeLength
  
  /**
   * The current "magic" value
   */
  val CurrentMagicValue: Byte = 0

  /**
   * Specifies the mask for the compression code. 2 bits to hold the compression codec.
   * 0 is reserved to indicate no compression
   */
  val CompressionCodeMask: Int = 0x03 

  /**
   * Compression code for uncompressed messages
   */
  val NoCompression: Int = 0

}
```

