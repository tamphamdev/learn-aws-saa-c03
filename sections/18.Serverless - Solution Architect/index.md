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

    ### Customization At The Edge

- Many modern applications execute some form of the logic at the edge
- **Edge Function:**
    - A code that you write and attach to CloudFront distributions
    - Run close to your users to minimize latency
- CloudFront provides two types: **CloudFront Functions** & **Lambda@Edge**
- You don’t have to manage any servers, deployed globally

### CloudFront Functions & Lambda@Edge Use Cases

- Website Security and Privacy
- Dynamic Web Application at the Edge
- Search Engine Optimization (SEO)
- Intelligently Route Across Origin and Data Centers
- Bot Mitigation at the Edge
- Real-time Image Transformation
- A/B Testing
- User Authentication and Authorization
- User Prioritization

### CloudFront Functions

- Lightweight functions written in JavaScript
- For high-scale, latency-sensitive CDN customizations
- Sub-ms startup times, **millions of requests/second**
- Used to change Viewer requests and responses:
    - **Viewer Request**: after CloudFront receives a request from a viewer
    - **Viewer Response**: before CloudFront forwards the response to the viewer

### Lambda@Edge

- Lambda functions written in NodeJs or Python
- Scales to 1000s of requests/second
- Used to change CloudFront requests and responses:
    - **Viewer Request**: after CloudFront receives a request from a viewer
    - **Origin Request**: before CloudFront forwards the request to the origin
    - **Origin Response:** after CloudFront receives the response from the origin
    - **Viewer Response**: before CloudFront forwards the response to the viewer
- Author your functions in one AWS Region (us-east-1) then CloudFront replicates to its locations

### CloudFront Functions vs Lambda@Edge

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b98f2b1c-765d-4111-ad8a-28f013a82e36/Untitled.png)

### Use cases

### CloudFront Functions

- Cache key normalization
    - Transform request attributes (headers, cookies, query strings, URL) to create an optimal Cache Key
- Header manipulation
    - Insert/modify/delete HTTP headers in the request or response
- URL rewrites or redirects
- Request authentication & authorization
    - Create and validate user-generated token (eg, JWT) to allow/deny requests

### Lambda@Edge

- Longer execution time (several ms)
- Adjustable CPU or memory
- Your code depends on a 3rd libraries (eg. AWS SDK to access other AWS services)
- Network access to use external services for processing
- File system access or access to the body of HTTP requests

### CloudFront Functions vs Lambda@Edge

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b98f2b1c-765d-4111-ad8a-28f013a82e36/Untitled.png)

### Use cases

### CloudFront Functions

- Cache key normalization
    - Transform request attributes (headers, cookies, query strings, URL) to create an optimal Cache Key
- Header manipulation
    - Insert/modify/delete HTTP headers in the request or response
- URL rewrites or redirects
- Request authentication & authorization
    - Create and validate user-generated token (eg, JWT) to allow/deny requests

### Lambda@Edge

- Longer execution time (several ms)
- Adjustable CPU or memory
- Your code depends on a 3rd libraries (eg. AWS SDK to access other AWS services)
- Network access to use external services for processing
- File system access or access to the body of HTTP requests

### Lambda by default

- By default, your Lambda function is launched outside your own VPC (in an AWS-owned VPC)
- Therefore, it cannot access resources in your VPC (RDS, ElasticCache, internal ELB…)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ad20aec8-6749-4773-b19d-367c0c245029/Untitled.png)

### Lambda in VPC

- You must define the VPC ID, the Subnets and the Security Groups
- Lambda will create an ENI (Elastic Network Interface) in your subnets

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1708af00-d65c-4537-a21b-03d64ea6fbd2/Untitled.png)

### Lambda with RDS Proxy

- If Lambda functions directly access your database, they may open too many connections under high load
- RDS Proxy
    - Improve scalability by pooling and sharing DB connections
    - Improve availability by reducing by 66% the failover time and preserving connections
    - Improve security by enforcing IAM authentication and storing credentials in Secrets Manager
- **The Lambda function must be deployed in you VPC, because RDS Proxy is never publicly accessible**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f0a72294-f960-4c67-8ed7-5cde596e20ec/Untitled.png)

### Invoking Lambda from RDS & Aurora

- Invoke Lambda functions from within your DB instance
- Allows you to process **data events** from within a database
- Supported for **RDS for PostgreSQL and Aurora MySQL**
- **Must allow outbound traffic to your Lambda function** from within your DB instance (Public, NAT Gateway, VPC Endpoints)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c1460d86-4036-49a6-8244-2875ad6fa885/Untitled.png)

### RDS Event Notifications

- Notifications that tells information about the DB instance itself (created, stopped, start…)
- You don’t have any information about the data itself
- Subscribe to the following event categories: **DB instance, DB snapshot, DB Parameter Group, DB Security Group, RDS Proxy, Custom Engine Version**
- Near real-time events (up to 5 minutes)
- Send notifications to SNS or subscribe to events using EventBridge

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f3600287-83cb-4a04-a943-f12abfd98920/Untitled.png)

### Amazon DynamoDB

- Fully managed, highly available with replication across multiple AZs
- NoSQL database - not a relational database - with transaction support
- Scales to massive workloads, distributed database
- Millions of requests per seconds, trillions of row, 100s of TB of storage
- Fast and consistent in performance (single-digit millisecond)
- Integrated with IAM for security, authorization and administration
- Low cost and auto-scaling capabilities
- No maintenance or patching, alway available
- Standard & Infrequent Access (IA) Table Class

### DynamoDB - Basics

- DynamoDB is made of **Tables**
- Each table has a **Primary Key** (must be decided at creation time)
- Each table can have an infinite number of items (rows)
- Each item has **attributes** (can be added over time - can be null)
- Maximum size of an item is **400KB**
- Data types supported are:
    - **Scalar Types** - String, Number, Binary, Boolean, Null
    - **Document Types** - List, Map
    - **Set Types** - String Set, Number Set, Binary Set
- **Therefore, in DynamoDB you can rapidly evolve schemas**

### DynamoDB - Table Example

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9b9f5d29-fa90-4789-b98d-689ff6e64b0e/Untitled.png)

### DynamoDB - Read/Write Capacity Modes

- Control how you manage your table’s capacity (read/write throughput)
- **Provisioned Mode (default)**
    - You specify the number of reads/writes per second
    - You need to plan capacity beforehand
    - Pay for provisioned Read Capacity Units (RCU) & Write Capacity Units (WCU)
    - Possibility to add auto-scaling mode for RCU & WCU
- **On-Demand Mode**
    - Read/writes automatically scale up/down with your workloads
    - No capacity planning needed
    - Pay for what you use, more expensive ($$$)
    - Great for unpredictable workloads, steep sudden spikes