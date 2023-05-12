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