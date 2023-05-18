# Overview

## S3 - Encryption

### Amazon S3 - Object Encryption

- You can encrypt objects in S3 buckets using one of 4 methods
- **Server-side Encryption (SSE)**
    - **Server-side Encryption With Amazon S3-Managed Keys (SSE-S3) - Enabled by Default**
        - Encrypts S3 objects using keys handled, managed and owned by AWS
    - **Server-side Encryption with KMS Keys stored in AWS KMS (SSE-KMS)**
        - Leverage AWS Key Management Service (AWS KMS) to manage encryption keys
    - **Server-Side Encryption with Customer-Provided Keys (SSE-C)**
        - When you want to manage your own encryption keys
- **Client-Side Encryption**
- It’s important to understand which ones are for which situation for the exam

### Amazon S3 Encryption - SSE-S3

- Encryption using keys handled, managed, and owned by AWS
- Object is encrypted server-side
- Encryption type is **AES-256**
- Must set header **“x-amz-server-side-encryption”:”AES256”**
- **Enabled by default for new buckets & new objects**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/14465512-d263-4bc2-9afe-3335cd7797fb/Untitled.png)

### Amazon S3 Encryption - SSE-KMS

- Encryption using keys handled and managed by AWS KMS (Key Management Service)
- KMS advantages: user control + audit key usage using CloudTrail
- Object is encrypted server side
- Must set header **“x-amz-server-side-encryption”:”aws:kms”**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/36c50094-eec1-4ba4-b491-45351d64c457/Untitled.png)

### SSE-KMS Limitation

- If you use SSE-KMS, you may be impacted by the KMS limits
- When you upload, it calls t he **GenerateDataKey** KMS API
- When you download, it calls the Decrypt KMS API
- Count towards the KMS quota per second (5500, 10000, 30000 req/s based on region)
- You can request a quota increase using he Service Quotas Console
-

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/72603639-dcd4-4ee4-a8b9-b464ad4be77d/Untitled.png)

### Amazon S3 Encryption - SSE-C

- Server-side Encryption using keys fully managed by the customer outside of AWS
- Amazon S3 does **NOT** store the encryption key you provide
- **HTTPS must be used**
- Encryption key must provided in HTTP headers, for every HTTP request made

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d27cbe20-d7c2-4b67-81d4-49a2588d8dac/Untitled.png)

### Amazon S3 Encryption - Client-Side Encryption

- Use client libraries such as **Amazon S3 Client-Side Encryption Library**
- Clients must encrypt data themselves before sending to Amazon S3
- Clients must decrypts data themselves when retrieving from Amazon S3
- Customer fully manages the keys and encryption cycle

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb55b65f-7cdd-4117-9338-3b4362d53642/Untitled.png)

### Amazon S3 - Encryption in transit (SSL/TLS)

- Encryption in flight is also called SSL/TLS
- Amazon S3 exposes two endpoints:
    - **HTTP endpoint** - non encrypted
    - **HTTPS endpoint** - encryption in flight
- **HTTPS is recommended**
- **HTTPS is mandatory for SSE-C**
- Most clients would use the HTTPS endpoint by default

## CORS

- **Cross-Origin Resource Sharing (CORS)**
- **Origin = scheme (protocol) + host (domain) + port**
    - example: [https://www.example.com](https://www.example.com) (implied port is 443 for HTTPS, 80 for HTTP)
- **Web Browser** based mechanism to allow requests to other origins while visiting the main origin
- Same origin: [http://example.com/app](http://example.com/app) & http://example.com/app2
- Different origins: [https://www.example.com](https://www.example.com) & http://other.example.com
- The requests won’t be fulfilled unless the other origin allows for the requests, using **CORS** **Headers** (example: **Access-Control-Allow-Origin**)

### Amazon S3 - CORS

- If a client makes a cross-origin request on our S3 bucket, we need to enable the correct CORS headers
- It’s a popular exam question
- You can allow for a specific origin or for * (all origins)

## Amazon S3 - MFA Delete

- **MFA (Multi-Factor Authentication)** - force users to generate a code on a device (usually a mobile phone or hardware) before doing important operations on S3
- MFA will be required to:
    - Permanently delete an object version
    - Suspend Versioning on the bucket
- MFA won’t be required to:
    - Enable Versioning
    - List deleted versions
- To use MFA Delete, **Versioning must be enabled** on the bucket
- **Only the bucket owner (root account) can enable/disable MFA delete**

## S3 Access Logs

- For audit purpose, you may want to log all access to S3 buckets
- Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket
- That data can be analyzed using data analysis tools…
- The target logging bucket must be in the same AWS region
- **WARNING**:
    - **Do not** set your logging bucket to be the monitored bucket
    - It will create a logging loop and **your bucket will grow exponentially**

## S3 - Pre-signed URLS

- Generate pre-signed URLs using the **S3 Console**, **AWS CLI** or **SDK**
- **URL Expiration**
    - **S3 Console** - 1 min up to 720 mins (12 hours)
    - **AWS CLI** - configure expiration with *—expires-in* parameter in seconds (default 3600 secs, max 604800 secs ~ 168 hours)
- Users given a pre-signed URL inherit the permissions of the users that generated the URL for GET/PUT
- Examples:
    - Allow only logged-in users to download a premium video from your S3 bucket
    - Allow an ever-changing list of users to download files by generating URLs dynamically
    - Allow temporarily a user to upload a file to a precise location in your S3 bucket