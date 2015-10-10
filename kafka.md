## Kafka


### 简介

Kafka是一个分布式，分区，多分片的日志服务。它提供了一个消息队列服务，但是具有独特的设计。

### 安装

(1)下载源代码 

    $ wget http://mirrors.hust.edu.cn/apache/kafka/0.8.2.0/kafka_2.10-0.8.2.0.tgz
    $ tar -xzf kafka_2.10-0.8.2.0.tgz
    $ sudo mkdir /opt/kafka
    $ sudo cp -rf kafka_2.10-0.8.2.0/ /opt/kafka
    
(2)启动服务

启动ZooKeeper

	$ sudo /opt/kafka/bin/zookeeper-server-start.sh /opt/kafka/config/zookeeper.properties
	[2015-10-10 09:38:03,177] INFO Reading configuration from: /opt/kafka/config/zookeeper.properties (org.apache.zookeeper.server.quorum.QuorumPeerConfig)
	[2015-10-10 09:38:03,183] INFO autopurge.snapRetainCount set to 3 (org.apache.zookeeper.server.DatadirCleanupManager)
	[2015-10-10 09:38:03,183] INFO autopurge.purgeInterval set to 0 (org.apache.zookeeper.server.DatadirCleanupManager)
	[2015-10-10 09:38:03,183] INFO Purge task is not scheduled. (org.apache.zookeeper.server.DatadirCleanupManager)
	[2015-10-10 09:38:03,183] WARN Either no config or no quorum defined in config, running  in standalone mode (org.apache.zookeeper.server.quorum.QuorumPeerMain)
	[2015-10-10 09:38:03,217] INFO Reading configuration from: /opt/kafka/config/zookeeper.properties (org.apache.zookeeper.server.quorum.QuorumPeerConfig)
	[2015-10-10 09:38:03,218] INFO Starting server (org.apache.zookeeper.server.ZooKeeperServerMain)  
	
启动服务器

    $ sudo /opt/kafka/bin/kafka-server-start.sh /opt/kafka/config/server.properties
	[2015-10-10 10:01:34,730] INFO Verifying properties (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,779] INFO Property broker.id is overridden to 1234 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,780] INFO Property log.cleaner.enable is overridden to false (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,780] INFO Property log.dirs is overridden to /tmp/kafka-logs (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,780] INFO Property log.retention.check.interval.ms is overridden to 300000 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,781] INFO Property log.retention.hours is overridden to 168 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,781] INFO Property log.segment.bytes is overridden to 1073741824 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,781] INFO Property num.io.threads is overridden to 8 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,782] INFO Property num.network.threads is overridden to 3 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,782] INFO Property num.partitions is overridden to 1 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,782] INFO Property num.recovery.threads.per.data.dir is overridden to 1 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,783] INFO Property port is overridden to 9092 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,783] INFO Property socket.receive.buffer.bytes is overridden to 102400 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,783] INFO Property socket.request.max.bytes is overridden to 104857600 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,783] INFO Property socket.send.buffer.bytes is overridden to 102400 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,784] INFO Property zookeeper.connect is overridden to localhost:2181 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,784] INFO Property zookeeper.connection.timeout.ms is overridden to 6000 (kafka.utils.VerifiableProperties)
	[2015-10-10 10:01:34,839] INFO [Kafka Server 1234], starting (kafka.server.KafkaServer)
	[2015-10-10 10:01:34,842] INFO [Kafka Server 1234], Connecting to zookeeper on localhost:2181 (kafka.server.KafkaServer)
	[2015-10-10 10:01:34,854] INFO Starting ZkClient event thread. (org.I0Itec.zkclient.ZkEventThread)

(3) 创建一个主题topic

    sudo /opt/kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
    Created topic "test"
 
 我们可以通过此命令查看创建的主题

    sudo /opt/kafka/bin/kafka-topics.sh --list --zookeeper localhost:2181

(4) 生产者发送消息

kafka可以通过命令行客户端来进行输入,一行就是一个单独的消息

    sudo /opt/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
	[2015-10-10 10:13:30,730] WARN Property topic is not valid (kafka.utils.VerifiableProperties)
	this is a message
	this is another message

(5) 消费者消费消息

     sudo /opt/kafka/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning
    this is a message
    this is another message
    
 (6)设置多个代理集群
 
       $ sudo cp /opt/kafka/config/server.properties /opt/kafka/config/server-1.properties
       $ sudo cp /opt/kafka/config/server.properties /opt/kafka/config/server-2.properties
   修改这文件配置信息如下:
   
       config/server-1.properties:
           broker.id=1
           port=9093
           log.dir=/tmp/kafka-logs-1
 
    config/server-2.properties:
        broker.id=2
        port=9094
        log.dir=/tmp/kafka-logs-2    
        
   broker.id在集群中必须是独一无二的,我们必须重新设置端口和日志文件因为我们将这些broker运行在相同的机器上面
   接下来我们启动下面2个Node
   
       $ sudo /opt/kafka/bin/kafka-server-start.sh config/server-1.properties &
       $ sudo /opt/kafka/bin/kafka-server-start.sh config/server-2.properties &
       
   接下来,我们创建一个新主题,但是复制因子为3
   
      $  sudo /opt/kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 3 --partitions 1 --topic my-replicated-topic
      
现在我们已经有一个集群，我们怎么查看代理是否在运行，我们可以使用"describe topics" 命令

    sudo /opt/kafka/bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic my-replicated-topic

我们也可以通过相同的命令查看我们最原始创建的topic

	sudo /opt/kafka/bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic test
	Topic:test	PartitionCount:1	ReplicationFactor:1	Configs:
		Topic: test	Partition: 0	Leader: 1234	Replicas: 1234	Isr: 1234
		
接下来我们发布一些消息到我们新的topic

	$ sudo /opt/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic my-replicated-topic
	...
	my test message 1
	my test message 2
	^C 
	
接下来我们消费这些信息

    $ sudo /opt/kafka/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --from-beginning --topic my-replicated-topic
	...
	my test message 1
	my test message 2
	^C
	
接下来我们来测试容错性

	$ ps | grep server-1.properties
	7564 ttys002    0:15.91 /System/Library/Frameworks/JavaVM.framework/Versions/1.6/Home/bin/java...
	$kill -9 7564
	
主领导节点转为一个从节点.node1不再在实时复制中

    $ sudo /opt/kafka/bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic my-replicated-topic
    
 但是消费者依然可以读写数据    
 
    $ sudo /opt/kafka/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --from-beginning --topic my-replicated-topic
	...
	my test message 1
	my test message 2
	^C


   
   