## kafka关键名词
producer：生产者。

consumer：消费者。

topic: 消息以topic为类别记录,每一类的消息称之为一个主题(Topic)。为了提高吞吐量，每个消息主题又会有多个分区

broker：存储和处理消息的服务，以集群的方式运行,可以由一个或多个服务组成，每个服务叫做一个broker;消费者可以订阅一个或多个主题(topic),并从Broker拉数据,从而消费这些已发布的消息。   　每个消息（也叫作record记录,也被称为消息）是由一个key，一个value和时间戳构成。

## 1.kafka为什么要在topic里加入分区的概念？ topic是逻辑的概念，partition是物理的概念，对用户来说是透明的。

producer只需要关心消息发往哪个topic，而consumer只关心自己订阅哪个topic，并不关心每条消息存于整个集群的哪个broker。  为了性能考虑，如果topic内的消息只存于一个broker，那这个broker会成为瓶颈，无法做到水平扩展。

所以把topic内的数据分布到整个集群就是一个自然而然的设计方式。

Partition的引入就是解决水平扩展问题的一个方案。  

如同我在Kafka设计解析（一）里所讲，每个partition可以被认为是一个无限长度的数组，新数据顺序追加进这个数组。

物理上，每个partition对应于一个文件夹。一个broker上可以存放多个partition。

这样，producer可以将数据发送给多个broker上的多个partition，consumer也可以并行从多个broker上的不同paritition上读数据，实现了水平扩展  

## kafka topic 分区的原因主要是为了并发访问，增大吞吐量。
## 具体来讲，> 每一个topic均被分成多个互不重复的partition >，其中每个partition均被放在一个或多个broker服务器上，这样就提供了容错。如果数据在不同的patition上，那么他们的访问将可能由不同的broker服务器来完成，这样就实现了并发访问。*分区的信息储存在Zookeeper上。*
## 所以，分区是kafka用来提供并发访问控制的一个概念。


# Zookeeper 在 Kafka 中的作用
## 1、Broker注册
Broker是分布式部署并且相互之间相互独立，但是需要有一个注册系统能够将整个集群中的Broker管理起来，此时就使用到了Zookeeper。在Zookeeper上会有一个专门用来进行Broker服务器列表记录的节点：
/brokers/ids
每个Broker在启动时，都会到Zookeeper上进行注册，即到/brokers/ids下创建属于自己的节点，如/brokers/ids/[0...N]。
Kafka使用了全局唯一的数字来指代每个Broker服务器，不同的Broker必须使用不同的Broker ID进行注册，创建完节点后，每个Broker就会将自己的IP地址和端口信息记录到该节点中去。其中，Broker创建的节点类型是临时节点，一旦Broker宕机，则对应的临时节点也会被自动删除。

## 名词
同步副本列表 (即ISR)
复制因子： Kafka每个topic的partition有N个副本，其中N是topic的复制因子
## kafka创建副本的2种方式
同步复制：

              1.producer联系zk识别leader

              2.向leader发送消息

              3.leadr收到消息写入到本地log

              4.follower从leader pull消息

              5.follower向本地写入log

              6.follower向leader发送ack消息

              7.leader收到所有follower的ack消息

              8.leader向producer回传ack

异步复制：

              和同步复制的区别在于，leader写入本地log之后，

              直接向client回传ack消息，不需要等待所有follower复制完成。
