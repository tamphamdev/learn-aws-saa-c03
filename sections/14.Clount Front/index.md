# AWS CloudFront

## Introduction

- Content Delivery Network (CND)
- **Improves read performance, content is cached at the edge**
- Improves users experience
- 216 Point of Presence globally (edge locations)
- **DDoS protection (because worldwide), integration with Shield, AWS Web Application Firewall**

### Origins

- **S3 Bucket**
    - For distributing files and caching them at the edge
    - Enhanced security with CloudFront **Origin Access Control** (OAC)
    - OAC is replacing Origin Access Identity (OAI)
    - CloudFront can be used as an ingress (to upload files to S3)
- **Custom Origin (HTTP)**
    - Application Load Balancer
    - EC2 Instance
    - S3 Website (must first enable the bucket as a static S3 website)
    - Any HTTP backend you want

### ClountFront at a hight level

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/37a9a821-f054-4d97-98a4-8d9e230d9010/Untitled.png)

### ClountFront - S3 as an Origin

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cee8d175-57dd-44dc-afd3-035306fad996/Untitled.png)

### CloudFront vs S3 Cross Region Replication

- CloudFront:
    - Global Edge network
    - Files are cached for a TTL (maybe a day)
    - **Great for static content that must be available everywhere**
- S3 Cross Region Replication:
    - Must be setup for each region you want replication to happen
    - Files are updated in near real-time
    - Read only
    - **Great for dynamic content that needs to be available at low-latency in few regions**