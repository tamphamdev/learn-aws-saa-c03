# Overview

## Event Processing in AWS

### Lambda, SNS & SQS

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e2f39d9d-74b7-4787-9b9d-b743d4c2e15a/Untitled.png)

### Fan Out Pattern: deliver to multiple SQS

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/517e7023-dd3d-40dc-bbf5-553f95a5422c/Untitled.png)

### S3 Event Notifications

- S3:ObjectCreate, S3:ObjectRemoved,S3:ObjectRestore, S3:Replication…
- Object name filtering possible (*.jpg)
- Use case: generate thumbnails of images uploaded to S3
- Can create as many “S3 events” as desired
- S3 event notification typically deliver events in seconds but can sometimes take a minute or longer

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/74091d37-cd90-4841-8ee2-e7270b89a901/Untitled.png)

### S3 Event Notifications with AWS EventBridge

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5ca76425-1e71-4dcc-8bae-53251720e738/Untitled.png)

- **Advanced filtering** options with JSON rules (metadata, object size, name…)
- **Multiple Destinations - ex Step Functions, Kinesis Streams / Firehose…**
- **EventBridge Capabilities** - Archive, Replay Events, Reliable delivery

### Amazon EventBridge - Intercept API Calls

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6b637649-4318-4196-9fa0-51d9a91beb00/Untitled.png)

### API Gateway - AWS Service Integration Kinesis Data Streams example

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2d15480f-0995-4a38-948c-949f25c88222/Untitled.png)

## Caching Strategies

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d56a29b8-2b24-43b4-a530-991178b6682d/Untitled.png)

- Càng về sau thì càng tăng cost và latency
- Mỗi app đều có cache riêng (CloudFront, API gateway…)

## Blocking an IP address

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/44316d9e-90de-4d00-bcd2-0e9f269be942/Untitled.png)

- Security group **không có deny rules** chỉ có allow rules nên không thể chặn từng IP riêng biệt. Chỉ được định nghĩa subnet trong SG.
- Có thể triển khai NACL (Network Access Control List) để định nghĩ rules, nhanh, dễ, rẻ.

### Blocking an IP address - with an ALB

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/837a9f47-153d-4944-81ce-af2dc821c655/Untitled.png)

- ALB sẽ termination các request trước khi tới EC2
- Tốn ít cost hơn vì ALB sẽ kết nối với EC2 thông qua private network

### Blocking an IP address with an NLB

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3317406a-4823-4094-8188-d27197f2dc4f/Untitled.png)

### Blocking an IP address - ALB + WAF

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a9d76123-3d7c-4454-a05e-0618d3f8c29a/Untitled.png)

### Blocking an IP address - ALB, CloudFront WAF

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1221c2be-e271-4748-81ab-2b71c4d3b711/Untitled.png)

- **NACL ở đây vô dụng**, vì Public ALB sẽ nhận CloudFront IP không biết được client nào request

## High Performance Computing (HPC)

- The cloud is the perfect place to perform HPC
- You can create a very high number of resources in no time
- You can speed up time to results by adding more resources
- You can pay only for the systems you have used
- Perform genomics, computational chemistry, financial risk modeling, weather prediction, machine learning, deep learning, autonomous driving…
- Which services help perform HPC?




### Data Management & Transfer

- **AWS Direct Connect:**
    - Move GB/s of data to the cloud, over a private secure network
- **Snowball & Snowmobile**
    - Move PB of data to the cloud
- **AWS DataSync**
    - Move large amount of data between on-premises and S3, EFS, FSx for Windows

### Compute and Networking

- EC2 Instances:
    - CPU optimized, GPU optimized
    - Spot Instances / Spot Fleets for cost saving + Auto Scaling
- EC2 Placement Groups: **Cluster** for good network performance

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/15122847-3fbe-47f9-8892-3da76ae24fea/Untitled.png)

- **EC2 Enhanced Networking (SR-IOV)**
    - Higher bandwidth, higher PPS (packet per second), lower latency
    - Option 1: **Elastic Network Adapter (ENA)** up to 100Gbps
    - Option 2: Intel 82599 VF up to 10 Gbps - LEGACY
- **Elastic Fabric Adapter (EFA)**
    - Improved ENA for **HPC**, only works for Linux
    - Great for inter-node communications, **tightly** coupled workloads
    - Leverages Message Passing Interface (MPI) standard
    - **Bypasses the underlying Linux OS to provide low-latency, reliable transport**

### Storage

- Instance-attached storage:
    - **EBS**: scale up to 256,000 IOPS with io2 Block Express
    - **Instance Store**: scale to millions of IOPS, linked to EC2 instance, low latency
- Network Storage:
    - **Amazon S3**: large blob, not a file system
    - **Amazon EFS**: scale IOPS based on total size, or use provisioned IOPS
    - **Amazon FSx for Lustre**:
        - HPC optimized distributed file system, millions of IOPS
        - Back by S3

### Automation and Orchestration

- **AWS Batch**
    - **AWS Batch** supports multi-node parallel jobs, which enables you to run single jobs that span multiple EC2 instances
    - Easily schedule jobs and launch EC2 instances accordingly
- **AWS ParallelCluster**
    - Open-source cluster management tool to deploy HPC on AWS
    - Configure with text files
    - Automate creation of VPC, Subnet, cluster type and instance types
    - **Ability to enable EFA on the cluster (improves network performance)**

### Creating a highly available EC2 instance

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0a5e1dc1-253d-47b0-96b9-1ebb373ee304/Untitled.png)

- CloudWatch sẽ thông báo và trigger Lambda function
- Lambda sẽ thực thi attach ElasticIP vào EC2 instance mới đc launch

### Creating a highly available EC2 instance with an Auto Scaling Group

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9a97f1c2-8daa-4699-bfba-751da6e4cb1f/Untitled.png)

### Creating a highly available EC2 instance with ASG + EBS

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/870cabed-0e06-431b-9909-5cba0f40ac19/Untitled.png)