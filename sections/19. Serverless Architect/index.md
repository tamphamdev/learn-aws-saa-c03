# Serverless Architect

# Content

## Mobile Application: MyTodoList

- We want to create a mobile application with the following requirements
- Expose as REST API with HTTPS
- Serverless architecture
- Users should be able to directly interact with their own folder in S3
- Users should authenticate through a managed serverless service
- The users can write and read-to-dos, but they mostly read them
- The database should scale, and have some high read throughput

### REST API layer

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f221b7ca-9c2e-4688-84e1-f423aa6c02f6/Untitled.png)

### Access to S3

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/13ba926d-b647-480a-9202-48babe87b3d9/Untitled.png)

### High read throughput, static data

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f1cfcd25-464b-4c7e-aa54-d99d721f4050/Untitled.png)

### Caching at the API Gateway

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7dc5edbb-0160-413f-b916-419755bb93ef/Untitled.png)

### In this lecture

- Serverless REST API: HTTPS, API Gateway, Lambda, DynamoDB
- Using Cognito to generate temporary credentials with STS to access S3 bucket with restricted policy, App users can directly access AWS resources this way. Pattern can be applied to DynamoDB, Lambda…
- Caching the reads on DynamoDB using DAX
- Caching the REST requests at the API Gateway level
- Security for authentication and authorization with Cognito, STS

## Serverless hosted website: Myblog.com

- This website should scale globally
- Blogs are rarely written, but often read
- Some of the website is purely static files, the rest is a dynamic REST API
- Caching must be implement where possible
- Any new users that subscribes should receive a welcome email
- Any photo uploaded to the blog should have a thumbnail generated

### Serving static content, globally

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e31665bb-06a8-4d3f-bc9e-f6389880b580/Untitled.png)

### Serving static content, globally, securely

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bc0a2ea6-18b8-4759-9961-a0d6e528b945/Untitled.png)

### Leveraging DynamoDB Global Tables

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/377f0f56-e94b-41e0-aca0-350e5cba0c9c/Untitled.png)

### User Welcome email flow

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dafd04b5-8b36-40cd-a0ba-0fa6e6e12eb6/Untitled.png)

### Thumbnail Generation flow

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b8f59d21-2a27-4299-816f-a2355c88d2e5/Untitled.png)

### AWS Hosted Website Summary

- We’ve seen static content being distributed using CloudFront with S3
- The REST API was serverless, didn’t need Cognito because public
- We leveraged a Global DynamoDB table to serve the data globally
- (we could have used Aurora Global Database)
- We enabled DynamoDB streams to trigger a Lambda function
- The Lambda function had an IAM role which could use SES
- SES (Simple Email Service) was used to send emails in a serverless way
- S3 can trigger SQS/SNS Lambda to notify of events