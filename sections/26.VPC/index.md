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