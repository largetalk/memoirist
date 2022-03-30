## ZkCient

ZkClient是由Datameer的工程师开发的开源客户端，对Zookeeper的原生API进行了包装，实现了超时重连、Watcher反复注册等功能；

  节点事件（数据事件），对应的事件处理器是IZKDataListener；\
  子节点事件，对应的事件处理器是IZKChildListener； \
  Session事件，对应的事件处理器是IZKStatusListener；


Zookeeper原始API的三个方法： getData, getChild, exists, ZKClient都提供了相应的代理方法

ZKClient提供了订阅功能，一个新建的会话，只需要在取得响应的数据节点后，调用subscribeXXX就可以订阅上相应的事件了。
```
subscribeChildChanges(String, IZKChildListener)
subscribeDataChanges(String, IZKDataListener)
subscribeStateChanges(IZKStatusListener)
```

#### ZKClient的CRUD

创建
```
createPersistent
createPersistentSequential
createEphemeral
createEphemeralSequential
```

删除
```
delete
```

修改
```
writeData
updateDataSerialized
writeDataReturnStat
```

#### 客户端处理变更流程

1. 判断变更类型，变更类型分为state变更、ChildNode变更、NodeData变更；
2. 取出与path关联的Listeners，并为每一个Listener创建一个ZKEvent，将ZkEvent，将ZkEvent交给ZkEventThread处理；
3. ZkEventThread线程，拿到ZkEvent后，只需要调用ZkEvent的run方法进行处理就可以了，所以，具体的如何调用Listener，还要依赖于ZkEvent的run()实现


