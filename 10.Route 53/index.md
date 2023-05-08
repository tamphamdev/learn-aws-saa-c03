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