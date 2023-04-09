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








