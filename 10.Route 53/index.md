# Route 53
#

## DNS

### What is DNS?

- Domain Name System which translates the human friendly hostnames into the machine IP addresses
- [www.google.com](http://www.google.com) ⇒ 172.217.18.36
- DNS is the backbone of the internet
- DNS uses hierachical naming structure

### DNS Terminologies

- **Domain Registrar**: Amazon Route 53, Godaddy…
- **DNS records**: A, AAAA, CNAME, NS….
- **Zone File**: contains DNS records
- **Name Server**: resolves DNS queries (Authoritative or Non-Authoritative)
- **Top Level Domain (TLD)**: .com, .us, .in. .gov…
- **Second Level Domain (SLD)**: amazon.com, google.com…

## Route 53 - Records

- How you want to route traffic for a domain
- Each record contains:
    - **Domain/subdomain Name** - eg. example.com
    - **Record Type** - eg. A or AAAA
    - **Value** - eg. 12.34.56.78
    - **Routing Policy** - how Route 53 responds to queries
- Route 53 supports the following DNS record types:
    - (must know) A/ AAAA / CNAME / NS
    - (advanced) CAA / DS / MX / NAPTR / PTR / SOA / TXT / SPF / SRV
### Route 53 - Record Types

- **A** - maps a hostname to IPv4
- **AAAA** - maps a hostname to IPv6
- **CNAME** - maps a hostname to another hostname
    - The target is a domain name which must have an A or AAAA record
    - Can’t create a CNAME record for the top node of a DNS namespace (Zone Apex)
    - Example: you can’t create for example.com, but you can create for www.example.com
- **NS** - NAme Server for the Hosted Zone
    - Control how traffic is routed for a domain
### Route 53 - Hosted Zones

- A containers for records that define how to route traffic to a domain and its subdomains
- **Public Hosted Zones** - contains records that specify how to route traffic on the Internet (public domain names) application1.mypublicdomain.com
- **Private Hosted Zones** - contain records that specify how you route traffic within one or more VPCs (private domain names) application1.company.internal
- You pay $0.50 per month per hosted zone

### Route 53 - Records TTL (Time To Live)

- **High TTL - eg. 24hr**
    - Less traffic on Route 53
    - Possibly outdated records
- **Low TTL - eg. 60 sec**
    - More traffic on Route 53 ($$)
    - Records are outdated for less time
    - Easy to change records
- **Except for Alias records, TTL is mandatory for each DNS record**

### CName vs Alias

- AWS Resources (Load balancer, CloudFront…) expose an AWS hostname:
    - [**abc.elb.amazonaws.com**](http://abc.elb.amazonaws.com) and you want **myapp.mydomain.com**
- CNAME:
    - Points a hostname to any other hostname. (app.mydomain.com ⇒ blabla.anything.com)
    - **ONLY FOR NON ROOT DOMAIN ( something.mydomain.com)**
- Alias:
    - Points a hostname to an AWS Resource (app.mydomain.com ⇒ blabal.amazonaws.com)
    - **Works for ROOT DOMAIN and NON ROOT DOMAIN (mydomain.com)**
    - Free of charge
    - Native health check

### Route 53 - Alias Records

- Maps a hostname to an AWS resource
- An extension to DNS functionality
- Automatically recognizes changes in the resources IP addresses
- Unlike CNAME, it can be used for the top node of a DNS namespace (Zone Apex): example.com
- Alias Record is always of type A/AAAA for AWS resources (IPv4/IPv6)

### Alias Records Targets

- Elastic Load Balancers
- CloudFront Distributions
- API Gateway
- Elastic Beanstalk environments
- S3 Websites
- VPC Interface Endpoints
- Global Accelerator accelerator
- Route 53 record in the same hosted zone
- **You cannot set an ALIAS record for an EC2 DNS name**

### Route 53 - Routing Policies

- Define how Route 53 responds to DNS queries
- Don’t get confused by the word “Routing”
    - It’s not the same as Load balancer routing which routes the traffic
    - DNS does not route any traffic, it only responds to the DNS queries
- Route 53 Supports the following Routing Policies
    - Simple
    - Weighted
    - Failover
    - Latency based
    - Geolocation
    - Multi-value answer
    - Geoproximity (using Route 53 Traffic Flow feature)

### Routing Policies - Simple

- Typically, route traffic to a single resource
- Can specify multiple values in the same record
- **If multiple values are returned, a random one is chosen by the client**
- When Alias enabled, specify only one AWS resource
- Can’t be associated with Health Checks

### Routing Policies - Weighted

- Control the % of the requests that go to each specific resource
- Assign each record a relative weight:
    - ***traffic (%)*** = *weight for a specific record* / *sum of all the weights for all records*
    - Weights don’t need to sum up to 100
- DNS records must have the same name and type
- Can be associated with Health Checks
- Use cases: loading balancing between regions, testing new application versions…
- **Assign a weight of 0 to a record to stop sending traffic to a resource**

### Routing Policies - Latency-based

- Redirect to the resource that has the least latency close to us
- Super helpful when latency for users is a priority
- **Latency is based on traffic between users and AWS Regions**
- Germany users may be directed to the US (if that’s the lowest latency)
- Can be associated with Health Checks (has a failover capacity)

### Route 53 - Health Checks

- HTTP Health Checks are only for **public resources**
- Health Check ⇒ Automated DNS Failover:
    - Health checks that monitor an endpoint (application, server, other AWS resource)
    - Health checks that monitor other health checks (Calculated Health Checks)
    - Health checks that monitor CloudWatch Alarm (full control!!) - eg: throttles of DynamoDB, alarms on RDS, custom metrics, ….(helpful for private resources)

### Health Checks - Monitor an Endpoint

- About 15 global health checkers will check the endpoint health
    - Healthy/Unhealthy Threshold - 3 (default)
    - Interval - 30 sec ( can set to 10 sec - higher cost)
    - Supported protocol: HTTP, HTTPS and TCP
    - If > 18% of health checkers report the endpoint is healthy, Route 53 considers it **Healthy**. Otherwise, it’s **Unhealthy**
    - Ability to choose which locations you want Route 53 to use

- Health Checks pass only when the endpoint responds with the 2xx and 3xx status codes
- Health Checks can be setup to pass / fail based on the text in the first **5120 bytes** of the response
- Configure you router/firewall to allow incoming requests from Route 53 Health Checkers