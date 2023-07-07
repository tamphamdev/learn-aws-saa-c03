# Overview

## CIDR

### Understanding CIDR IPv4

- **Classes Inter-Domain Routing** - a method for allocating IP addresses
- Used in **Security Groups** rules and AWS networking in general

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/50831fd4-924a-40d8-bc3f-82674341a8e9/Untitled.png)

- They help to define an IP address range:
    - We’ve see WW.XX.YY.ZZ/32 ⇒ one IP
    - We’ve seen 0.0.0.0/0 ⇒ all IPs
    - But we can define: 192.168.0.0/26 ⇒ 192.168.0.0 - 192.168.0.63 ( 64 IP addresses)
- A CIDR consists of two components
- **Base IP**
    - Represents an IP contained in the range (XX.XX.XX.XX)
    - Example: 10.0.0.0, 192.168.0.0…
- **Subnet Mask**
    - Defines how many bits can change in the IP
    - Example: /0, /24, /32
    - Can take two forms:
        - /8 ⇒ 255.0.0.0
        - /16 ⇒ 255.255.0.0
        - /24 ⇒ 255.255.255.0
        - /32 ⇒ 255.255.255.255


### Subnet Mask

- The Subnet Mask basically allow part of the underlying IP to get additional next values from the base IP

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8dca1799-7ab9-4d0d-b34a-7880d51b33c2/Untitled.png)

https://www.ipaddressguide.com/cidr ⇒ Link to check Subnet Mask

### Public vs Private IP (IPv4)

- The Internet Assigned Numbers Authority (IANA) established certain blocks of IPv4 addresses for the use of private (LAN) and public (Internet) addresses
- **Private IP** can only allow certain values:
    - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8) ⇒ in big networks
    - 172.16.0.0 = 172.31.255.255 (172.16.0.0/12) ⇒ **AWS default VPC in that range**
    - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16) ⇒ home networks
- All the rest of the IP addresses on the Internet are Public

## Default VPC

- All new AWS accounts have a default VPC
- New EC2 instances are launched into the default VPC if no subnet is specified
- Default VPC has Internet connectivity and all EC2 instances inside it have public IPv4 addresses
- We also get public and a private IPv4 DNS names

### VPC in AWS - IPv4

- VPC = Virtual Private Cloud
- You can have multiple VPCs in an AWS region( max. 5 per region- soft limit)
- Max CIDR per VPC is 5, for each CIDR:
    - Min size is /28 (16 IP addresses)
    - Max size is /16 (65536 IP addresses)
- Because VPC is private, only the Private IPv4 ranges are allowed:
    - 10.0.0.0 = 10.255.255.255 (10.0.0.0/8)
    - 172.1.6.0.0 - 172.1.255.255 (172.16.0.0/12)
    - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16)
- **Your VPC CIDR should NOT overlap with your other networks (ex: corporate)**

### State of Hands-on

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/018334e0-9656-44ac-85e1-dc5f805da108/Untitled.png)

### VPC - Subnet (IPv4)

- AWS reserves **5 IP addresses (first 4 and last 1**) in each subnet
- These 5 IP addresses are not available for use and can’t be assigned to an EC instance
- Example: if CIDR block 10.0.0.0/24, then reserved IP addresses are:
    - 10.0.0.0 - Network Address
    - 10.0.0.1 - reserved by AWS for the VPC router
    - 10.0.0.2 - reserved by AWS for mapping to Amazon-provided DNS
    - 10.0.0.3 - reserved by AWS for future use
    - 10.0.0.255 - Network Broadcast Address. AWS does not support broadcast in a VPC, therefore the address is reserved
- **Exam Tip**: if you need 29 IP addresses for EC2 instances:
    - You can’t choose a subnet of size /27 (32 IP addresses, 32- 5 = 27 < 29)
    - You need to choose a subnet of size /26 (64 IP addresses, 64 - 5 = 59 > 29)

## Internet Gateway (IGW)

- Allows resources (ex: EC2 instances) in a VPC connect to the Internet
- It scales horizontally and is highly available and redundant
- Must be created separately from a VPC
- One VPC can only be attached to one IGW and vice versa
- Internet Gateway on their own do not allow Internet access…
- Route table must also be edited!

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d90eb24-2e95-4552-971b-84a5d4ec236c/Untitled.png)

### Editing Route Tables

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9a31caa6-cdbd-40da-b1de-352396c41df9/Untitled.png)

## Bastion Hosts

- We can use a Bastion Host to SSH into our private EC2 instances
- The Bastion is in the public subnet which is then connected to all other private subnets
- **Bastion Host security group must allow** inbound from the internet on port 22 form restricted CIDR, for example the **public CIDR** of your corporation
- **Security Group of the EC2 Instances** must allow the Security Group of the Bastion Host, or the **private IP** of the Bastion host

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94b86938-f116-46c3-afcd-01567587eb10/Untitled.png)

## NAT Instance (outdated, but still at the exam)

- **NAT = Network Address Translation**
- Allows EC2 instances in private subnets to connect to the Internet
- Must be launched in a public subnet
- Must disable EC2 setting: Source / destination Check
- Must have Elastic IP attached to it
- Route Tables must be configured to route traffic from private subnets to the NAT Instance

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/457806f0-b238-4e7f-899d-0f27216eb292/Untitled.png)

### NAT Diagram

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/62f4cabc-fec5-4464-abe6-68e069b910b2/Untitled.png)

### NAT Instance - Comments

- Pre-configured Amazon Linux AMI is available
    - Reached the end of standard support on December 31, 2020
- Not highly available / resilient setup out of the box
    - You need to create an ASG in multi-AZ + resilient user-data script
- Internet traffic bandwidth depends on EC2 instance type
- You must manage Security Groups & rules:
    - Inbound:
        - Allow HTTP / HTTPS traffic coming from Private Subnets
        - Allow SSH from your home network (access is provided through Internet Gateway)
    - Outbound:
        - Allow HTTP / HTTPS traffic to the internet

## NAT Gateway

- AWS-managed NAT, higher bandwidth, high availability, no administration
- Pay per hour for usage and bandwidth
- NATGW is created in a specific Availability Zone, uses an Elastic IP
- Can’t be used by EC2 instance in the same subnet (only from other subnets)
- Requires an IGW (Private Subnet ⇒ NATGW ⇒ IGW)
- 5Gbps of bandwidth with automatic scaling up to 45Gbps
- No Security Groups to manage / require

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1a090cd7-12c3-4cf0-a50f-fbca4acc63b3/Untitled.png)

### NAT Gateway with High Availability

- NAT Gateway is resilient (đàn hồi) within a single Availability Zone
- Must create multiple NAT Gateways in multiple AZs for fault-tolerance
- There is no cross-AZ failover needed because if an AZ goes down it doesn’t need NAT

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5869d84f-c508-412c-9edc-9d05ef8be468/Untitled.png)

### NAT Gateway vs. NAT Instance

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ba5a238-8115-4c3a-9209-e820813895fe/Untitled.png)

### Network Access Control List (NACL)

- NACL are like a firewall which control traffic from and to **subnets**
- **One NACL per subnet**, new subnets are assigned the **Default NACL**
- You define **NACL Rules:**
    - Rules have a number (1-32766), higher precedence with a lower number
    - First rule match will drive the decision
    - Example: if you define #100 ALLOW 10.0.0.10/32 and #200 DENY 10.0.0.10/32, the IP address will be allowed because 100 has a higher precedence over 200
    - That last rule is an asterisk (*) and denies a request in case of no rule match
    - AWS recommends adding rules by increment of 100
- Newly created NACLs will deny everything
- NACL are a great way of blocking a specific IP address at the subnet level

### NACLs

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/088d7d3f-0717-4676-b364-acf4cdef5f87/Untitled.png)

### Default NACL

- Accepts everything inbound/outbound with the subnets it’s associated with
- Do **NOT** modify the Default NACL, instead create custom NACLs

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/01a55630-10ad-4feb-84f2-7272bc7de882/Untitled.png)

### Ephemeral Ports

- For any two endpoints to establish a connection, they must use ports
- Clients connect to a **defined port**, and expect a response on an **ephemeral port**
- Different Operating Systems use different port ranges, examples:
    - IANA & MS Windows 10 ⇒ 49152 - 65535
    - Many Linux Kernels ⇒ 32768 - 60999

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cd5f4657-1725-4c9e-b378-324dc75436a1/Untitled.png)

### NACL with Ephemeral Ports

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b771bb9e-39a0-4bd2-9944-cac3da9c68d6/Untitled.png)

### Create NACL rules for each target subnets CIDR

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa605ee6-ef65-4182-b3c0-4fb853c53683/Untitled.png)

### Security Group vs NACLs

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87f5b8f0-ba10-445f-ab2b-3d52e920bd1a/Untitled.png)

## VPC Peering

- Privately connect two VPCs using AWS network
- Make them behave as if they were in the same network
- Must not have overlapping CIDRs
- VPC Peering connection is NOT transitive (must be established for each VPC that need to communicate with one another)
- **You must update route tables in each VPC’s subnets to ensure EC2 instances can communicate with each other**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6f76ee7d-544c-493d-99e1-f747769f10c9/Untitled.png)

### Good to know

- You can create VPC Peering connection between VPCs in **different AWS accounts/regions**
- You can reference a security group in a peered VPC (works cross accounts - same region)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3e309c1b-3d4e-4eeb-97aa-2456a7076e52/Untitled.png)

### Diagram

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/24e46872-1463-4c29-902e-9b15634e96f9/Untitled.png)

## VPC Endpoints

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3baecfa7-b446-4646-b96d-55ff68fe3854/Untitled.png)

### AWS PrivateLink

- Every AWS service is publicly exposed (public URL)
- VPC Endpoints (powered by AWS Private Link) allows you to connect to AWS services using a private network instead of using the public Internet
- They’re redundant and scale horizontally
- They remove the need of IGW, NATGW,…. to access AWS Services
- In case of issues:
    - Check DNS Setting Resolution in your VPC
    - Check Route Tables

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8b8cb36a-41f3-4de5-8d07-80cab40ab05e/Untitled.png)

### Types of Endpoints

- **Interface Endpoints (powered by PrivateLink)**
    - Provisions an ENI (private IP address) as an entry point (must attach a Security Group)
    - Supports most AWS services
    - $ per hour + $ per GB of data processed
- **Gateway Endpoints**
    - Provisions a gateway and must be used **as a target in a route table (does not use security groups)**
    - Supports both S3 and DynamoDB
    - Free

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/65736469-c8dd-47e3-8584-770692175dcf/Untitled.png)


### Gateway or Interface Endpoint for S3?

- **Gateway is most likely going to be preferred all the time at the exam**
- Cost: free for Gateway, $ for interface endpoint
- Interface Endpoint is preferred access is required from on-premises (Site to Site VPC or Direct Connect), a different VPC or a different region

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2ab6921e-8da4-4128-997c-9fcc8ab0ddaa/Untitled.png)

## VPC Flow Logs

- Capture information about IP traffic going into your interfaces:
    - VPC Flow Logs
    - Subnet Flow Logs
    - Elastic Network Interface (ENI) Flow Logs
- Helps to monitor & troubleshoot connectivity issues
- Flow logs data can go to S3, CloudWatch Logs, and Kinesis Data Firehose
- Captures network information from AWS managed interfaces too: ELB, RDS, ElasticCache, RedShift, WorkSpaces, NATGW, Transit Gateway…

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/00f72512-be17-46f2-9865-453e89bc4442/Untitled.png)

### VPC Flow Logs Syntax

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04dfc14a-74b8-49c6-b463-23a031808141/Untitled.png)

- **srcaddr & dstaddr -** help identify problematic IP
- **srcport & dstport -** help identify problematic ports
- **Action** - success or failure of the request due to Security Group / NACL
- Can be used for analytics on usage patterns, or malicious behavior
- **Query VPC flow logs using Athena on S3 or CloudWatch Logs Insights**

### Troubleshoot SG & NACL issues

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d771ad3e-c641-406f-85a8-6811d215b317/Untitled.png)

### Architectures

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/867a26c5-fe00-48ff-b971-624b4ad8bc8d/Untitled.png)

## AWS Site-to-Site VPC

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1634ccdf-c362-48ee-8988-21c5256f298f/Untitled.png)

- **Virtual Private Gateway (VGW)**
    - VPN concentrator on the AWS side of the VPC connection
    - VGW is created an attached to the VPC from which you want to create the Site-to-Site- VPN connection
    - Possibility to customize the ASN (Autonomous System Number)
- **Customer Gateway (CGW)**
    - Software application or physical device on customer side of the VPC connection

### Site-to-Site VPN Connections

- Customer Gateway Device (On-premises)
    - What IP address to use?
        - Public Internet-routable IP address for your Customer Gateway device
        - If it’s behind a NAT device that’s enabled for ANT traversal (NAT-T), use the public IP address of the NAT device
- Important step: enable Route Propagation for the Virtual Private Gateway in the route table that is associated with your subnets
- If you need to ping your EC2 instances from on-premises make sure you add the ICMP protocol on the inbound of your security groups

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f38516c5-7558-4b34-9973-88f84b8b8564/Untitled.png)

### AWS VPN CloudHub

- Provide secure communication between multiple sites, if you have multiple VPC connections
- Low-cost hub-and-spoke model for primary or secondary network connectivity between different locations (VPC only)
- It’s a VPC connection so it goes over the public Internet
- To set it up, connect multiple VPC connections on the same VGW, setup dynamic routing and configure route tables

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6877d8e1-cf0c-40e1-a1fa-3245aa598e59/Untitled.png)

## Direct Connect (DX)

- Provides a dedicated **private** connection from a remote network to your VPC
- Dedicated connection must be setup between your DC and AWS Direct Connect locations
- You need to setup a Virtual Private Gateway on your VPC
- Access public resources (S3) and private (EC2) on same connection
- Use Cases:
    - Increase bandwidth throughput - working with large data sets - lower cost
    - More consistent network experience - applications using real-time data feeds
    - Hybrid Environments (on prem + cloud)
- Supports both IPv4 and IPv6

### Direct Connect Diagram

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/110fa233-c4d0-4f51-a395-4fe5fe3e1a55/Untitled.png)

### Direct Connect Gateway

- **If you want to setup a Direct Connect to one or more VPC in many different regions (same account), you must use a Direct Connect Gateway**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b1d30778-6f9c-420d-bf6d-f0ec1feb9697/Untitled.png)

### Direct Connect - Connection Types

- Dedicated Connections: 1Gbps, 10Gbps and 100 Gbps capacity
    - Physical ethernet port dedicated to a customer
    - Request made to AWS first, then completed by AWS Direct Connect Partners
- Hosted Connections: 50Mbps, 500Mbps, to 10Gbps
    - Connection requests are made via AWS Direct Connect Partners
    - Capacity can be **added or removed on demand**
    - 1, ,2, 5, 10 Gbps available at select AWS Direct Connect Partners
- Lead times a re often **longer than 1 month** to establish a new connection

### Direct Connect - Encryption

- Data in transit is not encrypted but is private
- AWS Direct Connect + VPC provides an IPsec-encrypted private connection
- Good for an extra level of security, but slightly more complex to put in place

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bbdecd9a-14be-403b-8422-4750d8f8826e/Untitled.png)

### Direct Connect - Resiliency

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d133a3cf-01b1-407c-85cf-1169d38b41fe/Untitled.png)

### Network topologies can become complicated

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca069044-d6e9-4d78-8259-61f7ef92e12f/Untitled.png)

## Transit Gateway

- For having transitive peering between thousands of VPC and on-premises, hub-and-spoke (star) connection
- Regional resource, can work cross-region
- Share cross-account using Resource Access Manager (RAM)
- You can peer Transit Gateways across regions
- Route Tables: limit which VPC can talk with other VPC
- Works with Direct Connect Gateway, VPN connections
- Supports **IP Multicast** (not supported by any other AWS service)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d4ec2ce2-18d0-4bb7-b686-40626e6d7f5d/Untitled.png)

### Transit Gateway: Site-to-Site VPN ECMP

- **ECMP = Equal-cost multi-path routing**
- Routing strategy to allow to forward a packet over multiple best path
- Use case: create multiple Site-to-Site VPN connection **to increase the bandwidth of your connection to AWS**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/05c473c9-3dde-4f26-aea7-2c9a608a4b28/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0004d80e-adab-44a3-9dfc-bbaba0272f78/Untitled.png)

### Transit Gateway - Share Direct Connect between multiple accounts

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3a9ca708-f518-4d74-a886-18ece07d966e/Untitled.png)

## VPC - Traffic Mirroring

- Allows you to capture and inspect network traffic in your VPC
- Route the traffic to security appliances that you manage
- Capture the traffic
    - **From (Source)** - ENIs
    - **To (Targets)** - an ENI or a Network Load Balancer
- Capture all packets or capture the packets of your interest (optionally, truncate packets)
- Source and Target can be in the same VPC or different VPCs (VPC Peering)
- Use cases: content inspection, threat monitoring, troubleshooting…

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/378d847f-b204-497e-8eb9-b47eb126d14e/Untitled.png)

## What is IPv6?

- IPv4 designed to provide 4.3 Billion addresses (they’ll be exhausted soon)
- IPv6 is the successor of IPv4
- IPv6 is designed to provide 3.4 x 10^38 unique IP addresses
- Every IPv6 address is public and Internet-routable (no private range)
- Format ⇒ x.x.x.x.x.x.x.x (x is hexadecimal, range can be from 0000 to ffff)
- Examples:
    - 2001:db8:3333:4444:5555:6666:7777:8888
    - 2001:db8:3333:4444:cccc:dddd:eeee:ffff
    - :: ⇒ 8 segments are zero
    - 2001:db8:: ⇒last 6 segments are zero
    - ::1234:5678 ⇒ first 6 segments are zero
    - 2001:db8::1234:5678 ⇒ middle 4 segments are zero

### IPv6 in VPC

- **IPv4 cannot be disabled for your VPC and subnets**
- You can enable IPv6 (they’re public IP addresses) to operate in dual-stack mode
- Your EC2 instances will get at least a private internal IPv4 and a public IPv6
- They can communicate using either IPv4 or IPv6 to the internet through an Internet Gateway

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/be23d0fa-91b1-4c97-ba46-94fd4b884c18/Untitled.png)

### IPv6 Troubleshooting

- **IPv4 cannot be disabled for your VPC and subnets**
- So, if you cannot launch an EC2 instance in your subnet
    - It’s not because it cannot acquire an IPv6 (the space is very large)
    - It’s because there are no available IPv4 in your subnet
- **Solution**: create a new IPv4 CIDR in your subnet

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/913d269d-d7e7-4d2a-b2b8-30b449a8af99/Untitled.png)

## Section Summary

- **CIDR** - IP Range
- **VPC** - Virtual Private Cloud ⇒ we define a list of IPv4 & IPv6 CIDR
- **Subnets** - tied to an AZ, we define a CIDR
- **Internet Gateway** - at the VPC level, provide IPv4 & IPv6 Internet Access
- **Route Tables** - must be edited to add routes from subnets to the IGW, VPC Peering Connections, VPC Endpoints…
- **Bastion Host** - public EC2 instance to SSH into, that has SSH connectivity to EC2 instances in private subnets
- **NAT Instances** - gives Internet access to EC2 instances in private subnet. Old, must be setup in a public subnet, disable Source/Destination check flag
- **NAT Gateway** - managed by AWS, provides scalable Internet access to private EC2 instances, IPv4 only
- **Private DNS + Route 53** - enable DNS Resolution + DNS Hostnames (VPC)
- **NACL** - stateless, subnet rules for inbound and outbound, don’t forget Ephemeral Ports
- **Security Groups** - stateful, operate at the EC2 instance level
- **Reachability Analyzer** - perform network connectivity testing between AWS resources
- **VPC Peering** - connect two VPCs with non overlapping CIDR, non-transitive
- **VPC Endpoints** - provide private access to AWS Services (S3, DynamoDB, CloudFormation, SSM) within a VPC
- **VPC Flow Logs** - can be setup at the VPC / Subnet / ENI Level, for ACCEPT and REJECT traffic, helps identifying attacks, analyze using Athena or CloudWatch Logs Insights
- **Site-to-Site VPC** - setup a Customer Gateway on DC, a Virtual Private Gateway on VPC, and site-to-site VPN over public Internet
- **AWS VPN CloudHub** - hub-and-spoke VPN model to connect your sites
- **Direct Connect** - setup a Virtual Private Gateway on VPC, and establish a direct private connection to an AWS Direct Connect Location
- **Direct Connect Gateway** - setup a Direct Connect to many VPCs in different AWS regions
- **AWS PrivateLink/VPC Endpoint Services:**
    - Connect services privately from your service VPC to customer VPC
    - Doesn’t need VPC Peering, public Internet, NAT Gateway, Route Tables
    - Must be used with Network Load Balancer & ENI
- **ClassicLink** - connect EC2-Classic EC2 instances privately to your VPC
- **Transit Gateway** - transitive peering connections for VPC, VPN & DX
- **Traffic Mirroring** - copy network traffic from ENIs for further analysis
- **Egress-only Internet Gateway** - like a NAT Gateway, but for IPv6

## Network Protection on AWS

- To protect network on AWS, we’ve seen
    - Network Access Control List. (NACLs)
    - Amazon VPC security groups
    - AWS WAF (protect against malicious requests)
    - AWS Shield & AWS Shield Advanced
    - AWS Firewall Manager (to mange. them across accounts)
- But what if we want to protect in a sophisticated way our entire VPC?

### AWS Network Firewall

- Protect your entire Amazon VPC
- From Layer 3 to Layer 7 protection
- Any direction, you can inspect
    - VPC to VPC traffic
    - Outbound to internet
    - Inbound from internet
    - To/from Direct Connect & Site-to-site VPN
- Internally, the AWS Network Firewall uses the AWS Gateway Load Balancer
- Rules can be centrally managed cross-account by AWS Firewall Manager to apply to many VPCs

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1121d720-aa46-41d7-9417-b2ab414b47be/Untitled.png)

### Network Firewall - Fine Grained Controls

- Supports 1000s of rules
    - IP & port - example: 10,000s of IPs filtering
    - Protocol - example: block the SMB protocol for outbound communications
    - Stateful domain list rule groups: only allow outbound traffic to *.mycorp.com or third-party software repo
    - General pattern matching using regex
- **Traffic filtering: Allow, drop, or alert for the traffic that matches the rules**
- **Active flow inspection** to protect against network threats with intrusion-prevention capabilities (like Gateway Load Balancer, but all managed by AWS)

## AWS Backup

- Supports PITR for supported services
- On-Demand and Scheduled backups
- Tag-based backup policies
- You create backup policies known as **Backup Plans**
    - Backup frequency (every 12 hours, daily, weekly, monthly, cron expression)
    - Backup window
    - Transition Cold Storage (Never, Days, Weeks, Months, Years)
    - Retention Period (Always, Days, Weeks, Months, Years)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/96bb8d95-e9be-402e-b7ff-30afa82a7c57/Untitled.png)

### AWS Backup Vault Lock

- Enforce WORM (Write Once Read Many) state for all the backups that you store in your AWS Backup Vault
- Additional layer of defense to protect your backups against:
    - Inadvertent or malicious delete operations
    - Updates that shorten or alter retention periods
- Even the root user cannot delete backups when enabled

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/64f01615-4abd-4b01-8b43-66c50b302446/Untitled.png)

## AWS Application Discovery Service

- Plan migration projects by gathering information about on-premises data centers
- Sever utilization data and dependency mapping are important for migrations
- **Agentless Discovery (AWS Agentless Discovery Connector)**
    - VM inventory, configuration, and performance history such as CPU, memory, and disk usage
- **Agent-based Discovery (AWS Application Discovery Agent)**
    - System configuration, system performance, running processes, and details of the network connections between systems
- Resulting data can be viewed within AWS Migration Hub

### AWS Application Migration Service (MGN)

- *The “AWS evolution” of CloudEndure Migration, replacing AWS Service Migration Service (SMS)*
- Lift-and-shift (rehost) solution which simplify migrating applications to AWS
- Converts your physical, virtual, and cloud-based servers to run natively on AWS
- Supports wide range of platforms, Operation Systems and databases
- Minimal downtime, reduced costs

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f63b7bcb-a071-424e-a4e4-52b839edc602/Untitled.png)

## Transferring large amount of data into AWS

- Example: transfer 200TB of data in the cloud. We have a 100Mbps internet connection
- **Over the Internet / Site-to-Site VPN:**
    - Immediate to setup
    - Will take 200(TB)*1000(GB)*1000(MB)*8(Mb)/100 Mbps = 16,000,000s = 185d
- **Over direct connect 1Gbps:**
    - Long for the one-tine setup (over a month)
    - Will take 200(TB)*1000(GB)*8(Mb)/1 Gbps = 1,600,000s = 18.5d
- **Over Snowball:**
    - Will take 2 to 3 snowballs in parallel
    - Takes about 1 week for the end-to-end transfer
    - Can be combined with DMS
- **For on-going replication / transfers: Site-to-Site VPN or DX with DMS or DataSync**

## VMWare Cloud on AWS

- Some customers use VMWare Cloud to mange their on-premises Data Center
- They want to extend the Data Center capacity to AWS, but keep using the VMware Cloud software
- …Enter VMware Cloud on AWS
- Use cases:
    - Migrate your VMware vSphere-based workloads to AWS
    - Run your production workloads across VMware vSphere-based private, public, an hybrid cloud environments
    - Have a disaster recover strategy

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/186653a7-7a45-4791-9d03-b901cab52121/Untitled.png)