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