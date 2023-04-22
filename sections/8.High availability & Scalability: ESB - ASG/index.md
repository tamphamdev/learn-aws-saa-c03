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
### Type of load balancer on AWS

- AWS has 4 kinds of managed Load Balancers
- Classic Load Balancer (v1 - old generation) - 2009 - CLB
    - HTTP, HTTPS, TCP, SSL (secure TCP)
- Application Load Balancer (v2 - new generation) - 2016 - ALB
    - HTTP, HTTPS, Websocket
- Network Load Balancer (v2 - new generation) - 2018 - NLB
    - TCP, TLS (secure TCP), UDP
- Gateway Load Balancer - 2020 - GWLB
    - Operates at layer 3 (Network layer) - IP Protocol
- Overall, it is recommended to use the newer generation load balancers as they provide more features
- Some load balancers can be setup as **internal** (private) or **external** (public) ELBs
### Load Balancer Security Groups

- Load balancer allow traffic from user
    - HTTP port 80 0.0.0.0/
    - HTTPS port 443 0.0.0.0/
- EC2 instances only allows traffic from Load Balancer with HTTP and port 80. Source from Load balancer is Security Group

### Application Load Balancer (v2)

- Application load balancers is Layer 7 (HTTP)
- Load balancing to multiple HTTP applications across machines (target groups)
- Load balancing to multiple applications on the same machine (EC2 instance) (ex: containers)
- Support for HTTP/2 and Websocket
- Support redirects (from HTTP to HTTPS for example)
- Routing tables to different target groups:
    - Routing based on path in URL (example.com/**users** & example.com/**posts**)
    - Routing based on host name in URL **(one.example.com** & **other**.**example.com**)
    - Routing based on Query String, Headers (example.com/users?**id=123&order=false**)
- ALB are a great fit for micro services & container-based application (example: Docker & Amazon ECS)
- Has a port mapping feature to redirect to a dynamic port in ECS
- In comparison, we’d need multiple Classic Load Balancer per application

### Application Load Balancer (v2) Target Groups

- EC2 instances (can be managed by an Auto Scaling Group) - HTTP
- ECS task (managed by ECS itself) - HTTP
- Lambda functions - HTTP request is translated into a JSON event
- IP Address - must be private IPs
- ALB can route to multiple target groups
- Health checks are at the target group level

### Good to know

- Fixed hostname (XXX.region.elb.amazonaws.com)
- The application servers don’t see the IP of the client directly
    - The true IP of the client is inserted in the header X-Forwarded-For
    - We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto)

### Network Load Balancer (v2)

- Network load balancer (Layer 4) allow to:
    - **Forward TCP & UDP traffic to your instances**
    - Handle millions of request per seconds
    - Less latency ~ 100ms (vs 400ms for ALB)
- **NLB has one static IP per AZ, and supports assigning Elastic IP (helpful for whitelisting specific IP)**
- NLB are used for extreme performance, TCP or UDP traffic
- Not included in the AWS free tier

### Network Load Balancer (v2) - TCP (Layer 4) Based Traffic

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/86f2eed8-6510-4f30-902a-b2d2792e6c03/Untitled.png)

### Network Load Balancer (v2) - Target Groups

- **EC2 instances**
- **IP Addresses** - must be private IPs
- **Application Load Balancer**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2f2d8bca-67a0-43b3-a59c-1a08ab2ccc36/Untitled.png)

### Gateway Load Balancer

- Deploy, scale, and manage a fleet of 3rd party
network virtual appliances in AWS
- Example: Firewalls, Intrusion Detection and
Prevention Systems, Deep Packet Inspection
Systems, payload manipulation, ...
- Operates at Layer 3 (Network Layer) - IP
Packets
- Combines the following functions:
• **Transparent Network Gateway** - single entry/exit
for all traffic
• **Load Balancer** - distributes traffic to your virtual
appliances
Uses the **GENEVE** protocol on port **6081**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5afc4acd-5044-42c1-ba76-b0101eadf932/Untitled.png)

### Gateway Load Balancer - Target Groups

- **EC2 instances**
- **IP Addresses** - must be private IPs

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3485e664-c7a1-4994-8a60-ab0b22553cd5/Untitled.png)

### Sticky Sessions (Session Affinity)

- It is possible to implement stickiness so that the same client is always redirected to the same instance behind a load balancer.
- This works for **Classic Load Balancer**, **Application Load Balancer**, and **Network Load Balancer.**
- The ‘cookie’ used for stickiness has an expiration date you control
- Use case: make sure the user doesn’t lose his session data
- Enabling stickiness may bring imbalance to the load over the backend EC2 instances

### Sticky Sessions - Cookie Names

- **Application-based Cookies**
    - Custom cookie
        - Generated by the target
        - Can include any custom attributes required by the application
        - Cooke name must be specified individually for each target group
        - Don’t use `AWSALB`, `AWSALBAPP`, or `AWSALBTG` (reversed for use by the ELB)
    - Application cookie
        - Generated by the load balancer
        - Cookie name is `AWSALBAPP`
- **Duration-based Cookies**
    - Cookie generated by the load balancer
    - Cookie name is **`AWSALB`** for ALB, **`AWSELB`** for CLB