# Kafka

- Publish e subscribe

Kafka is divided in topics where each topic has n partition. This topics represent a set of messages to be read by a consumer, and the partition has a persistence function of this datas.

Furthermore, kafka has Clusters that has various Brokers, these brokers are servers that stores Topics and some specific partition of these topic

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F1e4ae4e2-8acb-4ca0-b6d6-f5a968431cb1%2FUntitled.png?table=block&id=03fcda7c-c820-4cac-a1a9-12404c3180cc&spaceId=03ba54d8-5f10-4e37-8769-3d9b44b864d8&width=2000&userId=056a022f-b9fe-4f3a-96a2-7ba9fe116edc&cache=v2)

### Current Offset

The offset is a simple integer number that is used by Kafka to maintain the current position of a consumer. That's it. The current offset is a pointer to the last record that Kafka has already sent to a consumer in the most recent poll. So, the consumer doesn't get the same record twice because of the current offset.

### Committed Offset

Now let us come to committed offset, this offset is the position that a consumer has confirmed about processing. What does that mean? After receiving a list of messages, we want to process it. Right? This processing may be just storing them into HDFS. Once we are sure that we have successfully processed the record, we may want to commit the offset. So, the committed offset is a pointer to the last record that a consumer has successfully processed. For example, the consumer received 20 records. It is processing them one by one, and after processing each record, it is committing the offset. We will see a code example of this in a while.So, in summary.

1. Current offset -> Sent Records -> This is used to avoid resending same records again to the same consumer.
2. Committed offset -> Processed Records -> It is used to avoid resending same records to a new consumer in the event of partition rebalance.

The committed offset is critical in the case of partition rebalance.In the event of rebalancing. When a new consumer is assigned a new partition, it should ask a question. Where to start? What is already processed by the previous owner? The answer to the question is the committed offset.

### Replication factor

The replication factor is a way to replicate partitions between brokers, for in case the information being lost in a server the message itself doesn`t compromise. Each Broker has a partition evaluated as master, this partition is the same as those that consumers read. If the cluster has a replication factor of two, and a broker with a random partition is down, another broker will be responsible for these data provider, becoming the master

The criticality and the replication factor are proportional, so as the criticality increases, the replication factor increase too

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/512d2fd4-3cfb-446d-93e0-062159dc283f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211117T135014Z&X-Amz-Expires=86400&X-Amz-Signature=4f95517583454482a5dbbf5bc423b0388858542944f18779469369f1a5d8573a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

### Consumer and Producer

When the consumer wants to read a topic, kafka is in charge of enabling these reading between the various brokers that stores a topic.

If a consumer read some topic that another consumer has read, this message will be already processed , because there is a competition system between consumers. if a system needs  two consumers reading the same message, a Group consumer can be create to enable this.

The ideal shape and most optimized is has a consumer for each partition, because in that way we can parallelize a real time reading.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/fa56a145-957e-4cb4-b968-b77a06f84093/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211117T135048Z&X-Amz-Expires=86400&X-Amz-Signature=a39b63cd9bdd8d284af33f3fbfbf477f5e2b4aacfd5396834bfd84bc05911549&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

# Kafka Commands

```jsx
// List topics
kafka-topics --list --bootstrap-server localhost:29092
// Create topic w 3 partition and replication factor of 1
kafka-topics --create --botstrap-server localhost:29092 --partitions 3 --replication-factor 1 --topic test-3-1
//Describe topic
kafka-topics --describe --bootstrap-server localhost:29092 --topic teste-3-1

```
