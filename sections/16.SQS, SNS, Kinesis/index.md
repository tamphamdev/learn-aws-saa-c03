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