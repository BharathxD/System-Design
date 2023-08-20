# Kafka Essentials

- Kafka is a message stream that holds the messages
- Internally kafka has topic
- Every topic has `n` partitions
- Message is sent to a topic and depending on the configured hash key it is put into a partition
- Within the `partition, messages are ordered`
  - `No Ordering guarantee across partitions`

### Limitation of Kafka

`#consumers = #partitions`

If let’s say, we have three partitions then we can only have 3 consumers (No messages would be sent to the other/extra consumers)

The biggest limitation of kafka is parallelism
