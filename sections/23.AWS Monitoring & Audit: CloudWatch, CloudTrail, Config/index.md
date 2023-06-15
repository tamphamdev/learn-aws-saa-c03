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