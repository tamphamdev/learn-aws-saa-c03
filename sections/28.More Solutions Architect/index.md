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