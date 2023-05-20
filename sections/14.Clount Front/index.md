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

### CloudFront at a hight level

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/37a9a821-f054-4d97-98a4-8d9e230d9010/Untitled.png)

### CloudFront - S3 as an Origin

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

    ### CloudFront - Geo Restriction

- You can restrict who can access your distribution
    - **Allowlist**: Allow your users to access your content only if they’re in one of the countries on a list of approved countries
    - **Blocklist**: Prevent your users from accessing your content if they’re in one of the countries on a lit of banned countries
- The “country” is determined using a 3rd party Geo-IP database
- Use case: Copyright Laws to control access to content

### CloudFront - Pricing

- CloudFront Edge location are all around the world
- The cost of data out per edge location varies

### CloudFront - Price Classes

- You can reduce the number of edge locations for cost reduction
- Three price classes:
    - Price Class All: all regions - best performance
    - Price Class 200: most regions, but excludes the most expensive regions
    - Price Class 100: only the least expensive regions

### CloudFront - Cache Invalidations

- Im case you update the back-end origin, CloudFront doesn’t know about it and will only get the refreshed content after the TTL has expired
- However, you can force an entire or partial cache refresh (thus bypassing the TTL) by performing a **CloudFront Invalidation**
- You can invalidate all files (**) or a special path (/images/*)*

## AWS Global Accelerator

### Global users for our application

- You have deployed an application and have global users who want to access it directly
- They go over the public internet, which can add a lot of latency due to many hops
- We wish to go as fast as possible through AWS network to minimize latency

### Unicast IP vs Anycast IP

- Unicast IP: one server holds one IP address
- Anycast IP: all servers hold them same IP address and the client routed to the nearest one

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9d2a77f1-a9e3-4a67-a1d4-21f17318ddf5/Untitled.png)

### AWS Global Accelerator

- Leverage the AWS internal network to route to your application
- **2 Anycast IP** are created for your application
- The Anycast IP send traffic directly to Edge Locations
- The Edge Locations send the traffic to your application

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/71649389-9f5a-4816-b4bf-10f5f72464ea/Untitled.png)

- Works with **Elastic IP, EC2 Instances, ALB, NLB, public or private**
- Consistent Performance
    - Intelligent routing to lowest latency and fast regional failover
    - No issue with client cache (because the IP doesn’t change)
    - Internal AWS network
- Health Checks
    - Global Accelerator performs a health check of your applications
    - Helps make your application global (failover less than 1 minute for unhealthy)
    - Great for disaster recovery (thanks to the health checks)
- Security
    - Only 2 external IP need to be whitelisted

### AWS Global Accelerator vs CloudFront

- They both use the AWS global network and its edge locations around the world
- Both services integrate with AWS Shield for DDoS protection

- **CloudFront**
    - Improves performance for both cacheable content (such as images and videos)
    - Dynamic content (such as API accelerator and dynamic site delivery)
    - Content is served at the edge
- **Global Accelerator**
    - Improves performance for a wide range of applications over TCP or UDP
    - Proxying packets at the edge to application running in one or more AWS Regions.
    - Good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP