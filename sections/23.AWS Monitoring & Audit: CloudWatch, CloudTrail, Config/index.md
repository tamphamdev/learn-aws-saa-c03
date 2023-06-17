# Overview

## CloudWatch

- CloudWatch provides metrics for every service in AWS
- **Metric** is a variable to monitor (CPUUtilization, NetworkIn…)
- Metrics belong to **namespaces**
- **Dimension** is an attribute of a metric (instance id, environment, etc…)
- Up to 30 dimensions per metric
- Metrics have **timestamps**
- Can create CloudWatch dashboards of metrics
- Can create **CloudWatch Custom Metrics** (for the RAM for example)

### CloudWatch Metric Streams

- Continually stream CloudWatch metrics to a destination of your choice, with **near-real-time delivery** and low latency
    - Amazon Kinesis Data Firehose (and then its destinations)
    - 3rd party service provider: Datadog, Dynatrace, New Relic, Splunk, Sumo Logic….
- Option to filter metrics to only stream a subset of them

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fb26c11a-ae26-4ad8-8859-52ff20417b0f/Untitled.png)

### CloudWatch Logs

- **Log groups**: arbitrary name, usually representing an application
- **Log stream**: instances within application / log files / containers
- Can define log expiration policies (never expire, 1 day to 10 years…)
- **CloudWatch Logs can send logs to:**
    - Amazon S3 (export)
    - Kinesis Data Streams
    - Kinesis Data Firehose
    - AWS Lambda
    - OpenSearch
- Logs are encrypted by default
- Can setup KMS-based encryption with your own keys

### CloudWatch Logs - Sources

- SDK, CloudWatch Logs Agent, CloudWatch Unified Agent
- Elastic Beanstalk: collection of logs from application
- ECS: collection from containers
- AWS Lambda: collection from function logs
- VPC Flow Logs: VPC specific logs
- API Gateway
- CloudTrail based on filter
- Route53: Log DNS queries

### CloudWatch Logs Insights

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ccca743e-80bf-4eb2-a5d6-e90371d8d1ec/Untitled.png)

- Search and analyze log data stored in CloudWatch Logs
- Example: find a specific IP inside a log, count occurrences of “ERROR” in you logs…
- Provides a purpose-built query language
    - Automatically discovers fields from AWS services and JSON log events
    - Fetch desired event fields, filter based on conditions, calculate aggregate statistics, sort events, limit number of events…
    - Can save queries and ad them to CloudWatch Dashboards
- Can query multiple Log Groups in different AWS accounts
- It’s a query engine, not a real-time engine

### CloudWatch Logs - S3 Export

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3d7fe946-4f41-4aba-9036-3a99fe1f1837/Untitled.png)

- Log data can take **up to 12 hours** become available for export
- The API call is **CreateExportTask**
- Not near-real-time or real-time…use Logs Subscriptions instead

### CloudWatch Logs - Subscriptions

- Get a real-time log events from CloudWatch Logs for processing and analysis
- Send to Kinesis Data Streams, Kinesis Data Firehose, or Lambda
- **Subscription Filter** - filter which logs are events delivered to your destination

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/56b23247-07cc-4853-8cb0-57a1b199d9df/Untitled.png)

### CloudWatch Logs Aggregation Multi-Account & Multi Region

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/95e6cb99-c678-4590-8929-aa773d4eadb8/Untitled.png)

### CloudWatch Logs Subscriptions

- **Cross-Account Subscription** - send log events to resources in a different AWS account (KDS, KDF)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a43bcfb1-5e5f-41df-91b8-4d693203db82/Untitled.png)

### CloudWatch Logs for EC2

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d4df4ca0-39a6-4fed-886d-5ce8526df298/Untitled.png)

- By default, no logs from your EC2 machine will go to CloudWatch
- You need to run a CloudWatch agent on Ec2 to push the logs files you want
- Make sure IAM permissions are correct
- The CloudWatch log agent can be setup on-premises too

### CloudWatch Logs Agent & Unified Agent

- For virtual servers (EC2 instances, on-premises server…)
- **CloudWatch Logs Agent**
    - Old version of the agent
    - Can only send to CloudWatch Logs
- **CloudWatch Unified Agent**
    - Collect additional system-level metrics such as RAM, processes, etc…
    - Collect logs to send to CloudWatch Logs
    - Centralized configuration using SSM Parameter Store

### CloudWatch Unified Agent - Metrics

- Collected directly on your Linux server / EC2 instance
- **CPU** (active, guest, idle, system, user, steal)
- **Disk metrics** (free, used, total), Disk IO (writes, reads, bytes, iops)
- **RAM** (free, inactive, used, total, cached)
- **Netstat** (number of TCP and UDP connections, net packets, bytes)
- **Processes** (total, dead, bloqued, idle, running, sleep)
- **Swap Space** (free, used, used %)
- Reminder: out-of-the box metrics for EC2 - disk, CPI, network, (high level)

### CloudWatch Alarms

- Alarms are used to trigger notifications for any metric
- Various options (sampling, %, max, min, etc…)
- Alarm States:
    - OK
    - INSUFFICIENT_DATA
    - ALARM
- Period:
    - Length of time in seconds to evaluate the metric
    - High resolution custom metrics: 10 sec, 30 sec or multiples of 60 sec

### CloudWatch Alarm Targets

- Stop, Terminate, Reboot, or Recover an EC2 Instance
- Trigger Auto Scaling Action
- Send notification to SNS (from which you can do pretty much anything)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/819e69ec-8183-4128-aa68-96a938935672/Untitled.png)

### CloudWatch Alarms - Composite Alarms

- CloudWatch Alarms are on a single metric
- **Composite Alarms are monitoring the states of multiple other alarms**
- AND and OR conditions
- Helpful to reduce “alarm noise” by creating complex composite alarms

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0e0e7244-9dab-4d29-be9f-2957f2c7aeed/Untitled.png)

### EC2 Instance Recovery

- Status Check:
    - Instance status = check the EC2 VM
    - System status = check the underlying hardware

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d72862d7-a3c0-4fee-a453-ffe2a0ed9eaa/Untitled.png)

- **Recovery**: Same Private, Public, Elastic IP, metadata, placement group

### CloudWatch Alarm: good to know

- Alarms can be created based on CloudWatch Logs Metrics Filters

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2f7339ee-9649-412e-8282-e1380893a8f1/Untitled.png)

- To test alarms and notifications, set the alarm state to Alarm using CLI

### CloudWatch Container Insights

- Collect, aggregate, summarize, **metrics and logs** from containers
- Available for container on..
    - Amazon Elastic Containers Service (Amazon ECS)
    - Amazon Elastic Kubernetes Services (Amazon EKS)
    - Kubernetes platforms on EC2
    - Fargate (both for ECS and EKS)
- **In Amazon EKS and Kubernetes, CloudWatch Insights is using a containerized version of the CloudWatch Agent to discover containers**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/efe0a94d-d9a3-4483-8c8d-3db993a90576/Untitled.png)

### CloudWatch Lambda Insights

- Monitoring and troubleshooting solution for serverless applications running on AWS Lambda
- Collect, aggregates, and summarizes system-level metrics including CPI time, memory, disk and network
- Collects, aggregates, and summarizes diagnostic information such as cold starts and Lambda worker shutdowns
- Lambda Insights is provided as a Lambda Layer

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/afbb298f-ac55-42fe-81fe-286ea187989b/Untitled.png)

### CloudWatch Contributor Insights

- Analyze log data and create time series that display contributor data
    - **See metrics about the top-N contributors**
    - The total number of unique contributor, and their usage.
- This helps you find top talkers and understand who or what is impacting system performance
- Works for any AWS-generated logs (VPC, DNS, etc…)
- For example,  you can find bad hosts, **identify the heaviest network users**, or find the URLs that generate the most errors
- You can build your rules from scratch, or you can also use sample rules that AWS has created - **leverages your CloudWatch Logs**
- CloudWatch also provides built-in rules that you can use to analyze metrics from other AWS services.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eee48a62-4249-4c33-8fb9-1b92c0e6e991/Untitled.png)

### CloudWatch Application Insights

- **Provides automated dashboards that show potential problems with monitored applications, to help isolate ongoing issues**
- Your applications run on Amazon EC2 Instances with select technologies only (Java, .NET, databases….)
- And you can use other AWS resources such as Amazon EBS, RDS, ELB, ASG, Lambda, SQS, DynamoDB, S3 bucket, ECS, EKS, SNS, API Gateway…
- Powered by SageMaker
- Enhanced visibility into your application health to reduce the time it will take you to troubleshoot and repair your applications
- Findings and alerts are sent to Amazon EventBridge and SSM OpsCenter

### CloudWatch Insights and Operational Visibility

- **CloudWatch Container Insights**
    - ECS, EKS, Kubernetes on EC2, Farget, need agent for Kubernetes
    - Metrics and logs
- **CloudWatch Lambda Insights**
    - Detailed metrics to troubleshoot serverless applications
- **CloudWatch Contributors Insights**
    - Find “Top-N” Contributor through CloudWatch Logs
- **CloudWatch Application Insights**
    - Automatic dashboard to troubleshoot your application and related AWS services