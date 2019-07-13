## 启动zk服务端
> ./zkServer.sh start  
## 启动一个zk客户端连接服务端
> ./zkCli.sh -server 127.0.0.1:2181  

## 分离模式启动kafka服务端，使用server.properties配置文件
> ./kafka-server-start.sh ../config/server.properties &  (退出终端或者当前用户则任务结束，如关闭xshell连接断了，kafka服务也会结束)


>  nohup ./kafka-server-start.sh ../config/server.properties  & （永远后台运行，标准输出在当前目录nohup.out文件，不加& 不能接受标准输入）

## 创建一个topic,并指定分区个数和复制因子
> ./kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test

## 向topic发送消息
> ./kafka-console-producer.sh --broker-list localhost:9092 --topic test

## 消费消息
> ./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning

## 搭建集群
> config/server-1.properties:

    broker.id=1
    
    listeners=PLAINTEXT://192.168.17.128:9093   （vmware创建的虚拟机的ip，相当于内网ip）
    
    advertised.listeners=PLAINTEXT://192.168.1.8:9093 （客户端生产者和消费者访问kafka的ip，192.168.1.8相当于外网ip）
    
    log.dir=/tmp/kafka-logs-1
 
 
> config/server-2.properties:

    broker.id=2
    
    listeners=PLAINTEXT://192.168.17.128:9094
    
    advertised.listeners=PLAINTEXT://192.168.1.8:9094
    
    log.dir=/tmp/kafka-logs-2
