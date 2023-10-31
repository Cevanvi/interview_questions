### What is Kafka?

1) **High Throughput** - Support for millions of messages with modest hardware

2) **Scalability** - Highly scalable distributed systems with no downtime

3) **Replication** - Messages are replicated across the cluster to provide support for multiple subscribers and balances the consumers in case of failures

4) **Durability** - Provides support for persistence of message to disk

5) **Stream Processing** - Used with real-time streaming applications like Apache Spark & Storm
6) **Data Loss** - Kafka with proper configurations can ensure zero data loss

### Components in Kafka

1) Topic – a stream of messages belonging to the same type
2) Producer – that can publish messages to a topic
3) Brokers – a set of servers where the publishes messages are stored
4) Consumer – that subscribes to various topics and pulls data from the brokers.

### What is the optimal number of partitions for a topic?
The optimal number of partitions a topic should be divided into must be equal to the number of consumers.

### Explain the role of the offset.
Messages contained in the partitions are assigned a unique ID number that is called the offset. The role of the offset is to uniquely identify every message within the partition.

### What is a Consumer Group?
 Every Kafka consumer group consists of one or more consumers that jointly consume a set of subscribed topics.
 
### What is the role of the ZooKeeper?
Zookeeper stores all the metadata about offsets of messages consumed for a specific topic and partition by a specific Consumer Group.

### How can load balancing be ensured in Apache Kafka when one Kafka fails?
When a Kafka server fails, if it was the leader for any partition, one of its followers will take on the role of being the new leader to ensure load balancing. For this to happen, the topic replication factor has to be more than one,
i.e., the leader should have at least one follower to take on the new role of leader.

### Explain the concept of Leader and Follower.
Every partition in Kafka has one server(broker) which plays the role of a Leader, and none or more servers that act as Followers.
The Leader performs the task of all read and write requests for the partition, while the role of the Followers is to passively replicate the leader.
In the event of the Leader failing, one of the Followers will take on the role of the Leader.
This ensures load balancing of the server.
### What is Apache Kafka ack-value
An acknowledgment (ACK) is a signal passed between communicating processes to signify acknowledgment,
i.e., receipt of the message sent.
The ack-value is a producer configuration parameter in Apache Kafka and can be set to the following values:

1) **acks=0** The producer never waits for an ack from the broker when the ack value is set to 0. No guarantee can be made that the broker has received the message. The producer doesn’t try to send the record again since the producer never knows that the record was lost. This setting provides lower latency and higher throughput at the cost of much higher risk of message loss.
2) **acks=1** When setting the ack value to 1, the producer gets an ack after the leader has received the record. The leader will write the record to its log but will respond without awaiting a full acknowledgment from all followers. The message will be lost only if the leader fails immediately after acknowledging the record, but before the followers have replicated it. This setting is the middle ground for latency, throughput, and durability. It is slower but more durable than acks=0.
3) **acks=-1(all)** Setting the ack value to all means that the producer gets an ack when all in-sync replicas have received the record. The leader will wait for the full set of in-sync replicas to acknowledge the record. This means that it takes a longer time to send a message with ack value all, but it gives the strongest message durability.

### What is the ISR (In-Sync Replica)?
The ISR is simply all the replicas of a partition that are "in-sync" with the leader. The definition of "in-sync" depends on the topic configuration, but by default, it means that a replica is or has been fully caught up with the leader in the last 10 seconds. The setting for this time period is: replica.lag.time.max.ms and has a server default which can be overridden on a per topic basis.

At a minimum the, ISR will consist of the leader replica and any additional follower replicas that are also considered in-sync. Followers replicate data from the leader to themselves by sending Fetch Requests periodically, by default every 500ms.

If a follower fails, then it will cease sending fetch requests and after the default, 10 seconds will be removed from the ISR. Likewise, if a follower slows down, perhaps a network related issue or constrained server resources, then as soon as it has been lagging behind the leader for more than 10 seconds it is removed from the ISR.
###Why are Replications critical in Kafka?
Replication ensures that published messages are not lost and can be consumed in the event of any machine error, program error or frequent software upgrades.

### If a Replica stays out of the ISR for a long time, what does it signify?
It means that the Follower is unable to fetch data as fast as data accumulated by the Leader.

### In the Producer, when does QueueFullException occur?
QueueFullException typically occurs when the Producer attempts to send messages at a pace that the Broker cannot handle.
Since the Producer doesn’t block, users will need to add enough brokers to collaboratively handle the increased load.

### How long are messages retained in Apache Kafka?
Messages sent to Kafka are retained regardless of whether they are published or not for a specific period that is referred to as the retention period. The retention period can be configured for a topic. The default retention time is 7 days.

### What is the maximum size of a message that can be received by Apache Kafka?
The maximum size for a Kafka message, by default, is 1MB (megabyte). The size can be changed in the broker settings. However, Kafka is optimized to handle smaller messages of the size of 1KB.

### What is the role of the Partitioning Key?
Messages are sent to various partitions associated with a topic in a round-robin fashion. If there is a requirement to send a message to a particular partition, then it is possible to associate a key with the message. The key determines which partition that particular message will go to. All messages with the same key will go to the same partition. If a key is not specified for a message, the producer will choose the partition in a round-robin fashion.

### Explain fault tolerance in Apache Kafka.
In Kafka, the partition data is copied to other brokers, which are known as replicas. If there is a point of failure in the partition data in one node, then there are other nodes that will provide a backup and ensure that the data is still available. This is how Kafka allows fault tolerance.

### Mention some disadvantages of Apache Kafka

1) Tweaking of messages in Kafka causes performance issues in Kafka. Kafka works well in cases where the message does not need to be changed.

2) Kafka does not support wildcard topic selection. The exact topic name has to be matched.

3) In the case of large messages, brokers and consumers reduce the performance of Kafka by compressing and decompressing the messages. This affects the throughput and performance of Kafka.

4) Kafka does not support certain message paradigms like a point-to-point queue and client request/reply.

### What is __consumer_offsets
__consumer_offsets is used to store information about committed offsets for each topic:partition per group of consumers (groupID). It is compacted topic, so data will be periodically compressed and only latest offsets information available.

### related video : 
https://www.youtube.com/watch?v=-AZOi3kP9Js&t=1027s
