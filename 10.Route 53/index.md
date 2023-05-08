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