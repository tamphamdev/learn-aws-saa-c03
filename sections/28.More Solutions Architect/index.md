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