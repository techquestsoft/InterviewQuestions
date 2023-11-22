* https://docs.confluent.io/kafka/introduction.html
* https://developer.confluent.io/get-started/java/
* https://docs.confluent.io/platform/current/streams/index.html

* https://github.com/techquestsoft/kafka-java-getting-started

# 1. What producer Idempotent? 
    If teh meesgae received twice, it just overwrites the existing redord with 
    the same message data.
    Producer idempotence is automatically enabled with EOS to avoid duplicates 
    from producer retries.

# 2. Configuring Durability and Availability Guarantees

# 3. When Durability and Availability how latency will work?

# 4. Isolation levels in Consumer receipt

# 5. Transaction: Balance Overhead with Latency
        * Too few records per txn adds noticeable overhead
        * Too many recors per txn delays the exposure of output
        * Controllable through commit.interval.ms in KStreams

# 6. Interaction with External Systems:
        Automaic writes to kafka and external systems are not supported
            * Instead, write the transactinal output to a kafka topic first
            * Rely on idempotent to propagate the data from the output topic 
                to the external system. 

##### 7. https://www.confluent.io/blog/how-to-survive-a-kafka-outage/

##### 8. https://dzone.com/refcardz/apache-kafka-patterns-and-anti-patterns

##### 9. https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying

##### 10.  How to overcome message droppings or data packets missing in Kafka? 
````yaml
Message dropping or data packet loss in Apache Kafka can occur due to various reasons, 
and addressing this issue requires a combination of proper configuration, monitoring, 
and handling potential failure scenarios. Here are several strategies to overcome 
message droppings or data packets missing in Kafka.

1. Increase Replication Factor:
    Ensure that the replication factor for your Kafka topics is appropriately set. 
  This ensures that multiple copies (replicas) of each partition exist across different brokers. 
  If one broker goes down, another replica can serve the data.

2. Monitor Consumer Lag:
    Use tools like Kafka Manager, Confluent Control Center, or Burrow to monitor consumer lag. 
  Consumer lag represents the time it takes for a consumer to catch up with the latest messages. 
  Identifying and addressing lag can help prevent message dropping.

3. Configure Retention Policies:
    Set appropriate retention policies for your Kafka topics. If the retention period is too short, 
  messages may be deleted before consumers have a chance to consume them. If it's too long, 
  it may lead to unnecessary disk space consumption.

4. Tune Consumer Configuration:
    Adjust consumer configuration settings, such as auto.offset.reset and enable.auto.commit, 
  to ensure that consumers can recover from failures gracefully. For example, setting auto.offset.reset to "earliest" 
  can ensure that consumers start reading from the beginning of the topic if no offset is stored.

5. Implement Message Acknowledgment:
    Use Kafka's acknowledgment mechanism to ensure that messages are processed successfully before they are considered consumed. 
  This involves configuring your consumers to acknowledge messages only after processing.

6. Implement Message Replay and Dead Letter Queues:
    Consider implementing mechanisms for replaying messages in case of processing failures. 
  You can use dead-letter queues to store messages that couldn't be processed successfully and investigate the issues later.

7. Proper Resource Allocation:
    Ensure that Kafka brokers and consumers have sufficient resources (CPU, memory, disk) to handle the expected workload. 
  Insufficient resources can lead to performance issues and data loss.

8. Monitor and Alerts:
    Set up monitoring and alerts to be notified of any anomalies or issues in your Kafka cluster.
  Monitor metrics such as broker lag, under-replicated partitions, and resource utilization.

9. Regularly Update Kafka Version:
    Keep your Kafka version up to date. Newer versions often come with bug fixes, improvements, 
  and optimizations that can enhance stability and reliability.

10 Consider Using Exactly Once Semantics:
    Kafka provides exactly once semantics, which ensures that each message is processed by the consumer exactly once, 
  even in the presence of failures. This can be achieved using features like idempotent producers and transactions.

Remember that the specific steps to address message dropping can depend on the details of your Kafka deployment, 
  including your use case, workload, and cluster architecture. Regularly review and 
  update your configuration and procedures based on your evolving requirements.
````





