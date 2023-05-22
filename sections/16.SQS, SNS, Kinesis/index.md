# SQS, SNS, Kinesis, Active MQ

# Overview

## Section Introduction

- When we start deploying multiple applications, they will inevitably need to communicate with one another
- There are two patterns of application communication

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/03e0b97f-3197-4f16-8043-27282165204b/Untitled.png)

- Synchronous between applications can be problematic if there are sudden spikes of traffic
- What if you need to suddenly encode 1000 videos but usually it’s 10?

- In that case, it’s better to **decouple** your applications,
    - using SQS: queue model
    - using SNS: pub/sub model
    - using Kinesis: real-time streaming model
- These services can scale independently from our application

## Amazon SQS

### What’s a queue?

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d57d1025-8bf9-4513-8fbf-0b0ccd5b88f5/Untitled.png)

### Standard Queue

- Oldest offering (over 10 years old)
- Fully managed service, used to **decouple applications**

- Attributes:
    - Unlimited throughput, unlimited number of messages in queue
    - Default retention of messages: 4 days, maximum of 14 days
    - Low latency (<10ms on publish and receive)
    - Limitation of 256KB per message sent
- Can have duplicate messages (at least once delivery, occasionally)

### SQS - Producing Messages

- Produced to SQS using the SDK (SendMessage API)
- The message is **persisted** in SQS until a consumer deletes it
- Message retention: default 4 days, up to 14 days

- Example: send an order to be processed
    - Order id
    - Customer id
    - Any attributes you want
- SQS standard: unlimited throughput

### SQS - Consuming Messages

- Consumers (running on EC2 instances, servers, or AWS Lambda)…
- Poll SQS for messages (receive up to 10 messages at a time)
- Process the messages (example: insert the message into an RDS database)
- Delete the messages using the **DeleteMessage** API

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/26c67801-1d72-4a91-a3c2-99d5aefd23b0/Untitled.png)

### SQS - Multiple EC2 Instances Consumers

- Consumers receive and process messages in parallel
- At least once delivery
- Best-effort message ordering
- Consumers delete messages after processing them
- We can scale consumers horizontally to improve throughput of processing

### SQS with Auto Scaling Group (ASG)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a4dafa25-6d3f-4b5b-8786-36cc1222fd4c/Untitled.png)

### SQS to decouple between application tiers

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/86beb88e-01cd-4a3d-9f16-e65f4245405c/Untitled.png)

### Amazon SQS - Security

- **Encryption**:
    - In-flight encryption using HTTPS API
    - At-rest encryption using KMS keys
    - Client-side encryption if the client wants to perform encryption/decryption itself
- **Access Controls**: IAM policies to regulate access to the SQS API
- **SQS Access Policies** (similar to S3 bucket policies)
    - Useful for cross-account access to SQS queues
    - Useful for allowing other services (SNS, S3…) to write to an SQS queue

### SQS - Message Visibility Timeout

- After a message is polled by a consumer, it becomes invisible to other consumers
- Be default, the “message visibility timeout” is 30 seconds
- That means the message has 30 seconds to be processed
- After the message visibility timeout is over, the message is “visible” in SQS

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d058dbe3-7f44-47e2-915d-48438f784b4c/Untitled.png)

- If a message is not processed within the visibility timeout, it will be processed **twice**
- A consumer could call the **ChangeMessageVisibility** API to get more time
- If visibility timeout is high (hours), and consumer crashes, re-processing will take time
- If visibility timeout is too low (seconds), we may get duplicates

### SQS - Long Polling

- When a consumer requests messages from the queue, it can optionally “wait” for messages to arrive if there are none in the queue
- This is called Long Polling
- **LongPolling decrease the number of API calls made to SQS while increasing the efficiency and latency of your application**
- The wait time can be between 1 sec to 20 sec (20 sec preferable)
- Long Polling is preferable to Short Polling
- Long Polling can be enabled at the queue level or at the API level using **WaitTimeSeconds**

### SQS - FIFO Queue

- FIFO = First In First Out (ordering of messages in the queue)
- Limited throughput: 300msg/s without batching, 3000msg/s with batching
- Exactly-once send capacity (by removing duplicates)
- Messages are processed in order by the consumer

### SQS with Auto Scaling Group (ASG)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/645dac4f-abbc-4b98-bbf9-9b2a9e9c9383/Untitled.png)

### SQS as a buffer to database write

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1c0519f2-c19a-46d9-895b-e29cbf246547/Untitled.png)

### SQS to decouple between application tiers

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5ac42468-b037-44cb-8477-0fb87f89dab3/Untitled.png)

## Amazon SNS

- What if you want to send one message to many receivers?

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/701b9a0a-63c4-4990-a662-5d83fc41bf7f/Untitled.png)

### Amazon SNS - Simple Notification Service

- The “event producer” only sends message to one SNS topic
- As many “event receivers” (subscriptions) as we want to listen to the SNS topic notifications
- Each subscriber to the topic will get all the messages (note: new feature to filter messages)
- Up to 12,500,000 subscriptions per topic
- 100,000 topics limit

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/938c6492-2a94-4f5d-b11d-4fd0a9bd4241/Untitled.png)

### AWS SNS - How to publish

- Topic Publish (using the SDK)
    - Create a topic
    - Create a subscription (or many)
    - Publish to the topic
- Direct Publish (for mobile apps SDK)
    - Create a platform application
    - Create a platform endpoint
    - Publish to the platform endpoint
    - Works with Google GCM, Apple APNS, Amazon ADM…

### Amazon SNS - Security

- **Encryption**:
    - In-flight encryption using HTTPS API
    - At-rest encryption using KMS keys
    - Client-side encryption if the client wants to perform encryption/decryption itself
- **Access Controls**: **IAM policies to regulate access to the SNS API**
- **SNS Access Policies** (similar to S3 bucket policies)
    - Useful for cross-account access to SNS topics
    - Useful for allowing other services (S3…) to write to an SNS topic

### SNS + SQS : Fan out

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/349a9fad-58b3-4860-8d39-3f46b67afaee/Untitled.png)

- Push once in SNS. receive in all SQS queue that are subscribers
- Fully decoupled, data data loss
- SQS allows for: data persistence, delayed processing and retries of work
- Ability to add more SQS subscribers over time
- Make sure your SQS queue **access policy** allows for SNS to write
- Cross-Region Delivery: works with SQS Queues in other regions

### Application: S3 Events to multiple queues

- For the same combination of : **event type** (eg: object create) and **prefix** (eg: images/) you can only have one S3 Event rule
- IF you want to send the same S3 event to many SQS queues, use fan-out

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc28f5e0-74a9-4e16-8872-68e0a6887ef1/Untitled.png)

### Application: SNS to Amazon S3 through Kinesis Data Firehose

- SNS can send to Kinesis and therefore we can have the following solutions architecture:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4f09f08d-0da7-4be4-9619-b6fb252bc836/Untitled.png)

### Amazon SNS - FIFO Topic

- FIFO = First In First Out (ordering of messages in the topic)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aeed5f52-0b9b-4cf6-8556-a7df067cd8d0/Untitled.png)

- Similar features as SQS FIFO:
    - Ordering by Message Group ID (all messages in the same group are ordered)
    - Deduplication using a Deduplication ID or Content Based Deduplication
- **Can only have SQS FICO queues as subscribers**
- Limited throughput (same throughput as SQS FIFO)

### SNS - Message Filtering

- JSON policy used to filter messages sent to SNS topic’s subscriptions
- If a subscription doesn’t have filter policy, it receives every message

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9e4c0878-3454-4987-a52c-a7294abd4002/Untitled.png)

## Kinesis (streaming)

### Overview

- Makes it easy to collect, process, and analyze streaming data in real-time
- Ingest real-time dat such as: Application logs, Metrics, Website clickstreams, IoT telemetry data..

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/210c1015-0d62-4da7-af04-c407e61db840/Untitled.png)

- **Kinesis Data Streams**: capture, process and store data streams
- **Kinesis Data Firehose**: load data streams into AWS data stores
- **Kinesis Data Analytics**: analyze data streams with SQL or Apache Flink
- Kinesis Video Streams: capture, process and store video streams

### Kinesis Data Streams

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5c5242a-21da-449f-bf03-9199bdedeaa2/Untitled.png)

- Retention between 1 day to 36 days
- Ability to process (replay) data
- Once data is inserted in Kinesis, it can’t be deleted (immutability)
- Data that shares the same partition goes to the same shard (ordering)
- Producers: AWS SDK, Kinesis Producer Library (KPL), Kinesis Agent
- Consumers:
    - Write your own: Kinesis Client Library (KCL), AWS SDK
    - Managed: AWS Lambda, Kinesis Data Firehose, Kinesis Data Analytics

### Kinesis Data Streams - Capacity Modes

- Provisioned mode:
    - You choose the number of shards provisioned, scale manually or using APO
    - Each shard gets 1MB/s in (or 1000 records per second)
    - Each shard gets 2MB/s out (classic or enhanced fan-out consumer)
    - You pay per shared provisioned per hours
- On-demand mode:
    - No need to provision or manage the capacity
    - Default capacity provisioned (4Mb/s in or 4000 records per second)
    - Scales automatically based on observed throughput peak during the last 30days
    - Pay per stream per hour & data in/out per GB

### Kinesis Data Stream Security

- Control access / authorization using IAM policies
- Encryption in flight using HTTPS endpoints
- Encryption at rest using KMS
- You can implement encryption/decryption of data on client side (harder)
- VPC Endpoints available for Kinesis to access within VPC
- Monitor API calls using CloudTrail

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c64d5c81-c361-49fe-9eeb-78951da58fcc/Untitled.png)