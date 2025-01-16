---
title: Kafka for Beginners
layout: default
nav_order: 2
parent: Learning
---
# Kafka for Beginners

## What is Kafka

Kafka is a distributed streaming platform often used for building real-time data pipelines and streaming applications. It’s designed to handle high-throughput, fault-tolerant, and scalable event streaming. Here's an overview and a guide to learning Kafka effectively.

Official Documentation: [Kafka Documentation](https://kafka.apache.org/documentation/)

## Core Kafka Concepts

**1. Topics:** Categories to which records are sent by producers.\
**2. Partitions:** Subdivisions of topics for parallelism and scalability.\
**3. Producers:** Applications or processes that publish data to Kafka topics.\
**4. Consumers:** Applications or processes that read data from Kafka topics.\
**5. Brokers:** Kafka servers that manage storage and delivery of messages.\
**6. ZooKeeper:** Used for managing and coordinating Kafka brokers (deprecated in newer versions).\
**7. Kafka Connect:** Tool for streaming data between Kafka and other systems.\
**8. Kafka Streams:** Library for stream processing directly in Kafka.\

---
---
## 1. Topics

In Apache Kafka, a topic is a core abstraction that acts as a channel or category where producers send data, and consumers retrieve it. Topics are fundamental to Kafka's publish-subscribe messaging model.

- A topic is a category or feed name to which records are published.
- Topics are partitioned, and each partition is an immutable log of messages.
- Messages in a topic are ordered within a partition but not across partitions.


### Key Characteristics of Kafka Topics

**1. Name**
- Each topic has a unique name within a Kafka cluster.
- Topic names are case-sensitive and often represent the nature of the data being handled (e.g., user_events, order_transactions).

**2. Partitions**
- Topics are divided into partitions, which are the basic unit of parallelism and scalability.
- Each partition contains a sequence of messages that are ordered, immutable, and identified by a unique offset.
- Producers write data to a specific partition, and consumers read data from partitions.

~~~
Example: A topic with 3 partitions:

Topic: "user_activity"
Partitions:
- Partition 0: Messages 0, 1, 2, ...
- Partition 1: Messages 0, 1, 2, ...
- Partition 2: Messages 0, 1, 2, ...
~~~

**3. Replication:**
- Kafka retains messages in a topic for a configurable time period or until a certain size is reached.
- By default, messages are retained even after they have been consumed.


**4. Retention:**

- Kafka retains messages in a topic for a configurable time period or until a certain size is reached.
- By default, messages are retained even after they have been consumed.

**5. Log Structure:**

- Each partition is stored as a log file on the broker.
- Messages are appended to the log, and consumers read from the log sequentially.

### Types of Topic Usage Patterns

**1. Publish-Subscribe:**

- Multiple consumers can subscribe to a topic and process the same data independently.
> Example: Logging systems where multiple systems process the same log events.

**2. Load Balancing:**

- Each partition of a topic can be consumed by only one consumer in a consumer group.
- This allows for parallel processing and load distribution across multiple consumers.

### Configurable Properties of Topics
Kafka topics have several configuration options:

**1. Retention Period:**

- Determines how long messages are stored in a topic.
- Configurable via log.retention.ms or log.retention.bytes.

**2. Number of Partitions:**

- Configured at topic creation.
- More partitions allow for higher throughput and better parallelism.

**3. Replication Factor:**

- The number of copies of each partition.
- For high availability, this value should be greater than 1.

**4. Cleanup Policy:**

- Defines how old messages are handled:
    - *delete*: Removes old messages based on the retention period.
    - *compact*: Retains only the latest value for each key.

---
---
## 2. Partitions

In Kafka, partitions are fundamental to how data is stored, distributed, and processed. They are the building blocks of a Kafka topic, enabling scalability, parallelism, and fault tolerance.

- A partition is a segment or subdivision of a Kafka topic.
- Each topic is divided into one or more partitions to enable parallelism and scalability.
- Partitions are replicated across Kafka brokers for fault tolerance.
- Every partition has a leader broker and several follower brokers.
- Partitions allow Kafka to distribute and store data across multiple brokers in a Kafka cluster.
- Within a partition, messages are ordered sequentially by an offset.

### Key Characteristics of Partitions

**1. Uniqueness within a Topic:**
- Each partition in a topic is identified by a number, starting from 0 (e.g., Partition 0, Partition 1, etc.).

**2. Message Ordering:**

- Kafka guarantees message order within a partition. Messages are appended in the order they arrive.
- Across partitions, ordering is not guaranteed.

**3. Offsets:**

- Messages in a partition are assigned a unique offset, which is a monotonically increasing number.
- Consumers use offsets to track their progress in reading data.

**4. Scalability:**

- Partitions enable Kafka to distribute load among multiple brokers. Each partition can be hosted on a different broker, allowing for parallel processing.
- Producers and consumers can also scale by interacting with partitions independently.

**5. Replication:**

- Each partition has replicas on other brokers for fault tolerance.
- One replica is designated as the leader, and others are followers.
- Producers and consumers interact only with the leader.

### Partition Assignment

**1. By Default (Round-Robin):**
- If the producer doesn't specify a partition, Kafka assigns partitions in a round-robin manner for load balancing.

**2. Custom Partitioning:**
- Producers can define a partition key to decide which partition a message should go to.
- For example, a key like a user ID can ensure all messages related to that user go to the same partition, maintaining order for that key.

### Partitioning in the Kafka Workflow

**1. Producer Perspective:**

- A producer sends a message to a Kafka topic.
- Kafka determines the target partition based on:
    - A user-defined partitioning strategy.
    - The default round-robin approach (if no key is specified).

**2. Broker Perspective:**

- Brokers store partitions on their disk. Each broker hosts a subset of the partitions.
- The leader partition handles all read and write requests for that partition.

**3. Consumer Perspective:**

- Consumers subscribe to a topic and read messages from its partitions.
- If part of a consumer group, partitions are distributed among the group members, ensuring each partition is read by only one consumer in the group.

### Benefits of Partitions

**1. Parallelism:**

- Multiple partitions can be processed in parallel by producers and consumers, improving throughput.

**2. Scalability:**

- Adding more partitions allows the system to handle higher data volumes and distribute the load across more brokers.

**3. Fault Tolerance:**

- With replication, even if a broker hosting a partition fails, other brokers hosting replicas can take over.

> Kafka partitions are critical to its design, allowing for scalability, fault tolerance, and efficient data distribution. Each partition provides a way to break down a topic into smaller, manageable chunks, enabling Kafka to process vast amounts of data in a distributed and reliable manner.

