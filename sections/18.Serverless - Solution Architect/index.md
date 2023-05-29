# Serverless: Solution Architecture

# Content

## What’s serverless?

- Serverless is a new paradigm in which the developers don’t have to manage servers anymore…
- They just deploy code
- They just deploy…functions !
- Initially..Serverless == FaaS (Function as a Service)
- Serverless was pioneered by AWS Lambda but now also includes anything that’s managed: “databases, messaging, storage, etc”
- **Serverless does not mean there are no servers**…it means you just don’t manage / provision / see them

## Serverless in AWS

- AWS Lambda
- DynamoDB
- AWS Cognito
- AWS API Gateway
- Amazon S3
- AWS SNS & SQS
- AWS Kinesis Data Firehose
- Aurora Serverless
- Step Functions
- Fargate

## Lambda

Amazon EC2

- Virtual Server in the Cloud
- Limited by RAM and CPU
- Continuously running
- Scaling means intervention to add/remove servers

Amazon Lambda

- Virtual **Functions** - no servers to manage
- Limited by time - **short executions**
- Run **on-demand**
- **Scaling is automated**

### Benefits of AWS Lambda

- Easy Pricing:
    - Pay per request and compute time
    - Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time
- Integrated with the whole AWS suite of services
- Integrated with many programming languages
- Easy monitoring through AWS CloudWatch
- Easy to get more resources per functions (up to 10GB of RAM)
- Increasing RAM will also improve CPU and network

### AWS Lambda language support

- Node.js (Javascript)
- Python
- Java (Java 8 compatible)
- C# (.NET Core)
- Golang
- C# / PowerShell
- Ruby
- Custom Runtime API (Community supported, example Rust)
- Lambda Container Image
    - The container image must implement the Lambda Runtime API

### AWS Lambda Integrations Main ones

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/357b6941-1dc4-494e-8a72-8f0b41539ac2/Untitled.png)

### Example: Serverless Thumbnail creation

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3b68c79b-4a83-4d71-af19-253695e94a82/Untitled.png)

### Example: Serverless CRON Job

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d60f792-1285-4619-a893-763c6fbadefc/Untitled.png)

### AWS Lambda Pricing: example

- You can find overall pricing information here:  **https://aws.amazon.com/lambda/pricing**
- Pay per **calls**:
    - First 1,000,000 request are free
    - $0.20 per 1million requests thereafter ($0,0000002 per request)
- Pay per **duration**: (in increment of 1 ms)
    - 400,000 GB-seconds of compute time per month if FREE
    - == 400,000 seconds if function is 1GB RAM
    - == 3,200,000 second if function is 128MB RAM
    - After that $1.00 for 600,000 GB-seconds
- It is usually very cheap to run AWS Lambda so it’s very popular

### AWS Lambda Limits to Know - per region

- **Execution**:
    - Memory allocation: 128MB - 10GB (1MB increments)
    - Maximum execution time: 900 seconds (15minutes)
    - Environment variables (4KB)
    - Disk capacity in the “function container” (in /tmp): 512MB to 10GB
    - Concurrency executions: 1000 (can be increased)
- **Deployment**:
    - Lambda function deployment size (compressed .zip): 50MB
    - Size of uncompressed deployment (code + dependencies): 250MB
    - Can use the /tmp directory to load other files at startup
    - Size of environment variables: 4KB