ZooKeeper 是一个用于分布式应用的分布式协调服务(Distributed Coordination Service)，它提供了一组原语集，分布式应用程序可以在这些原语的基础上实现更高级别的同步、配置维护、分组和命名服务。
> 原语(primitives)： 操作系统或计算机网络用语，是指由若干条指令组成，用于完成一定功能的过程。具有不可分割性，即原语的执行必须是连续的，在执行过程中不允许被中断。

## 数据模型(Data model)
ZooKeeper的数据模型也叫分层命名空间，它类似一个标准文件系统，底层是一个多叉树，命名空间中的每个节点都代表一个路径。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35016921/1676014114042-95ad14c3-67c7-490a-8580-6d261116dc1a.png#averageHue=%23f4f4f4&clientId=u07093090-cc8d-4&from=paste&id=ua1aa1317&name=image.png&originHeight=253&originWidth=442&originalType=url&ratio=1.25&rotation=0&showTitle=false&size=32642&status=done&style=none&taskId=u09d21455-cd3c-43c5-9f8d-3f5a4110fac&title=)
## 数据节点(znode)
ZooKeeper中每个数据节点被称为znode，是ZooKeeper中数据的最小单元。
#### znode的类型：

- 持久节点（PERSISTENT）：一旦创建就一直存在，直到将其删除，即使ZooKeeper集群宕机。
- 临时节点（EPHEMERAL）：临时节点的生命周期与客户端会话绑定，会话断开则节点消失。并且临时节点只能做叶子节点 ，不能创建子节点。
- 持久顺序节点（PERSISTENT_SEQUENTIAL）：除了具有持久节点的特性之外，节点名称还具有顺序性。
- 临时顺序节点（EPHEMERAL_SEQUENTIAL）：除了具有临时节点的特性之外，节点名称还具有顺序性。
#### znode的数据结构：
每个 znode 由 2 部分组成：stat表示状态信息；data存放的数据内容。
stat的结构如下：

| 字段名 | 作用 |
| --- | --- |
| czxid | create zxid，即该数据节点被创建时的事务id |
| mzxid | modified zxid，即该节点最终一次更新时的事务id |
| pzxid | 该节点的子节点列表最后一次修改时的事务id，只有子节点列表变更才会更新pzxid，子节点内容变更不会更新 |
| ctime | create time，即该节点的创建时间 |
| mtime | modified time，即该节点最后一次的更新时间 |
| version | 节点版本号，节点创建时为0，每更新一次节点内容该版本号的值增加1 |
| cversion | 子节点版本号，当前节点的子节点每次变化时值增加1 |
| aversion | 节点的ACL版本号，表示该节点ACL信息变更次数 |
| ephemeralOwner | 创建该临时节点时会话的sessionId；如果当前节点为持久节点，则ephemeralOwner=0 |
| dataLength | 数据节点内容长度 |
| numChildren | 当前节点的子节点个数 |

## 事件监听器(watcher)
ZooKeeper允许用户在指定节点上注册一些Watcher，并且在特定事件触发的时候，ZooKeeper服务端会将事件通知到注册的客户端上去，该机制是ZooKeeper实现分布式协调服务的重要特性。
![](https://cdn.nlark.com/yuque/0/2023/png/35016921/1676016953879-797e0433-7235-49e2-aa22-f35d7b8b2b43.png#averageHue=%23fbfaf9&clientId=u07093090-cc8d-4&from=paste&id=u9f7c255f&originHeight=317&originWidth=1082&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=u3dec6d1e-9bea-4bcc-bcf2-b7fd87eb695&title=)

## ZooKeeper集群角色
ZooKeeper 集群中有 Leader、Follower 和 Observer 三种角色。
![](https://cdn.nlark.com/yuque/0/2023/png/35016921/1676017554089-7ed8b6aa-fb40-4905-b363-9da84996af71.png#averageHue=%23f9f8f5&clientId=u07093090-cc8d-4&from=paste&id=u82517c07&originHeight=290&originWidth=700&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=u8a517eb7-a033-4d81-80d4-b65946add9a&title=)
ZooKeeper 集群中的所有机器通过一个 Leader 选举过程来选定一台称为“Leader”的机器，Leader 既可以为客户端提供写服务又能提供读服务。除了 Leader 外，Follower 和 Observer 都只能提供读服务。Follower 和 Observer 唯一的区别在于 Observer 机器不参与 Leader 的选举过程，也不参与写操作的“过半写成功”策略，因此 Observer 机器可以在不影响写性能的情况下提升集群的读性能。
Leader选举过程如下：

1. Leader election（选举阶段）：节点在一开始都处于选举阶段，只要有一个节点得到超半数节点的票数，它就可以当选准 leader。
2. Discovery（发现阶段） ：在这个阶段，followers 跟准 leader 进行通信，同步 followers 最近接收的事务提议。
3. Synchronization（同步阶段）：同步阶段主要是利用 leader 前一阶段获得的最新提议历史，同步集群中所有的副本。同步完成之后准 leader 才会成为真正的 leader。
4. Broadcast（广播阶段）：到了这个阶段，ZooKeeper 集群才能正式对外提供事务服务，并且 leader 可以进行消息广播。同时如果有新的节点加入，还需要对新节点进行同步。

