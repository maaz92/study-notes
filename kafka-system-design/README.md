# Kafka For System Design Interviews
## _My Notes On Kafka For System Design Interviews_

Kafka is used by many top tech companies as a streaming solution. Kafka is too big to learn or memorize all of it.

## Problems that kafka solves
Let's consider a problem where we have multiple requests coming in to run a long asynchronous process. 
1. As the number of requests increases, a fixed number of servers won't be able to handle all the requests.
2. There is no way to ensure that the requests get processed in a fixed order.
3. For real time processing the requests must be processed with minimum delay.
## Terminologies
#### Broker: Ther server (physical or virtual) that holds the queue.
#### Partition: The "queue". An ordered immutable sequence of messages that we append to like a log file. Each broker can have miltiple partitions.
#### Topic: The logical grouping of partitions. You publish to and consume from topics in Kafka.
#### Producer: Writes messages/records to topics.
#### Consumer: Reads messages/records off of topics.

## Structure of a kafka message/record
```
Header
Key
Value
Timestamp
```
## Publishing message to a kafka broker and a topic
You can use a kafka client to connect and publish message to a kafka broker and a topic.
Details: The server guarantees that on a single TCP connection, requests will be processed in the order they are sent and responses will return in that order as well. The broker's request processing allows only a single in-flight request per connection in order to guarantee this ordering. Note that clients can (and ideally should) use non-blocking IO to implement request pipelining and achieve higher throughput. i.e., clients can send requests even while awaiting responses for preceding requests since the outstanding requests will be buffered in the underlying OS socket buffer. All requests are initiated by the client, and result in a corresponding response message from the server except where noted.

The server has a configurable maximum limit on request size and any request that exceeds this limit will result in the socket being disconnected.

#### 1. Publish message to kafka topic
#### 2. If message has a partition key then hash(key)%N is calculated to identify the correct partition
#### 3. If message has doesn't have a partition key then a broker is chosen using round robin or other logic
#### 4. Identifies the broker for the partition.
#### 5. Sends message to the broker.
#### 6. Broker appends message to the correct partition. The appended message will have an associated offset for the partition.

## Consuming message
- Consumers read next message based on offsets.
- Periodically commit the offset to kafka broker
- Incase consumer goes down or network breaks then after the consumer is back up then it starts reading from the last committed offset.

## How does a Kafka cluster work
1. Kafka cluster consists of 1 or more brokers
2. Each broker contains 1 or more partition replicas
3. Each partition has 1 leader replica and 0 or more follower replicas

#### Leader Replica
1. Is responsible for all the read and write operations for a partition
2. Assignment of leader replica for a partition is controlled by some central controller. Which ensures that each partitions central controller is essentially distributed accross the whole cluster to balance the load. 

#### Follower Replicas
1. Follower replicas of a partition can reside on the same or different broker as the leader replica of the partition.
2. Followers don't handle read or write requests. They just passively replicate messages from the leader replica. They act like a backup ready to become a leader if the leader goes down.

#### Consumers
1. Consumer subscribes to a topic and reads all messages from that group.
2. Consumers can be grouped into consumer groups. Each message from a topic is consumed by a consumer group only once.

## When to use Kafka in an interview?
1. Any time you need a messaging queue.
- Processing can be done async
- In-order message processing
- Pub-sub i.e. message need to be processed by multiple consumers simultaneously
Application servers for I/O can be scaled horizontally to handle bursts. Processing/Computing servers can be kept behind a messaging queue.

## Scalability
#### Constraints:
1. Aim for <1MB per message. There's no limit from Kafka on the size of a message but to avoid networks issues and unsure optimal performance. Otherwise, network or memory will be overwhelmed.
- Avoid huge message sizes
2. 1 broker upto 1TB data and 10k messages per second.

#### How to scale:
1. More brokers
2. Chose a good partition key
Cloud providers OR Managed Kafka can handle scaling.

#### How to handle hot partition(too many messages)
1. Remove the key so message distributes evenly across brokers
2. Compound key so message are further categorized/partitioned but no ordering for the previous partition key
3. Backpressure: Slow down the producer

#### Relevant Settings
1. **acks**  Acks is the number of acknowldgments to consider a message published)
acks=all means that the Producer waits for ack from all in-sync replicas (ISR) (leader + followers). Ensures maximum durability.
possible values={0,1,-1ORall}
2. Replication factor, 3 is default. Is the number of replicas for any partition

#### What happens when a consumer goes down
1. Just read from the latest offset
2. If a consumer in a consumer group goes down then rebalance. (Each consumer group is responsible for a partition range)

#### Producer configs
1. Retries
2. InitialRetryTime
3. Idempotent=true

#### Producer optimizations
1. Batch messages in producer
2. Compress messages in producer

#### Retention policy
1. retention.ms (default 7 days) How long kafka retains a message
2. retention.bytes (default 16GB) When to start purging based on size










