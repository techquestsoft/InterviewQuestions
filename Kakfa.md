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








