# Scalability & High Availability

## Scalability & Hight Availability

- Scalability means that an application / system can handle greater loads by adapting.
- There are two kinds of scalability:
    - Vertical Scalability
    - Horizontal Scalability (= elasticity)
- **Scalability is linked but different to High Availability**

### Vertical Scalability

- Vertical scalability means **increasing the size of the instance**
- For example, your application runs on a t2.micro
- Scaling that application vertically means running it on a t2.large
- Vertical scalability is very common for non distributed systems, such as a database.
- RDS, ElasticCache are services that can scale vertically.
- There’s usually a limit to how much you can vertically scale (hardware limit)

### Horizontal Scalability

- Horizontal Scalability means **increasing the number of instances** / systems for your application
- Horizontal scaling **implies distributed systems.**
- This is very common for web applications/ modern applications
- It’s easy to horizontally scale thanks the cloud offering such as Amazon EC2.

### High Availability

- High Availability usually goes hand in hand with horizontal scaling
- High availability means running your application / system in at least 2 data centers (== AZ)
- The goal of high availability is to survive a data center loss
- The high availability can be passive (for RDS Multi AZ for example)
- The high availability can be active (for horizontal scaling)

### High Availability & Scalability for EC2

- Vertical Scaling: increase instance size (= scale up/down)
    - From: t2.nano - 0.5G of RAM, 1 vCPU
    - To: u-12tb 1.metal - 12.3TB of RAM, 448 vCPU
- Horizontal Scaling: Increase number of instances (= scale out/in)
    - Auto scaling group
    - Load balancer
- High Availability: Run instances for the same application across multi AZ
    - Auto Scaling Group multi AZ
    - Load Balancer multi AZ
## Load Balancing

### Overview

- Load balances are servers that forward traffic to multiple servers (eg. EC2 instances) downstream

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e7afef2c-29fb-43f0-aebc-a9e997c7cd1a/Untitled.png)

- Why use a load balancer?
    - Spread load across multiple downstream instances
    - Expose a single point of access (DNS) to your application
    - Seamlessly handl failure of downstream instances
    - Do regular heath checks to your instances
    - Provide SSL termination (HTTPS) for your websites
    - Enforce stickiness with cookies
    - High availability across zones
    - Separate public traffic from private traffic
    - An Elastic Load Balancer is a **managed load balancer**
        - AWS guarantees that it will be working
        - AWS takes care of upgrades, maintenance, high availability
        - AWS provides only a few configuration knobs
    - It costs less to setup your own load balancer but it will be a lot more effort on your end
    - It is integrated with many AWS offering / services
        - EC2, EC2 Auto Scaling Groups, Amazon ECS
        - AWS Certificate Manager (ACM), CloudWatch
        - Route 53, AWS WAF, AWS Global Accelerator
### Health Checks

- Heath checks are crucial for Load Balancers
- They enable the load balancer to know if instances it forwards traffic to are available to reply to requests
- The heath check is done on a port and a route (/health is common)
- If the response is not 200 (OK), then the instance is unhealthy