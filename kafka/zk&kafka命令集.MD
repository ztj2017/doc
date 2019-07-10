> cd bin
> ./zkServer.sh start  启动zk服务端
> ./zkCli.sh -server 127.0.0.1:2181  启动一个zk客户端连接服务端

## 分离模式启动kafka服务端，使用server.properties配置文件
> ./kafka-server-start.sh ../config/server.properties & 

## 创建一个topic,并指定分区个数和复制因子
> ./kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test

## 向topic发送消息
> ./kafka-console-producer.sh --broker-list localhost:9092 --topic test

## 消费消息
> ./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
