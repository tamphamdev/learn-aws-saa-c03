# Overview

KMS, Encryption SDK, SSM Parameter Store

## Encryption

### Encryption in flight (SSL)

- Data is encrypted before sending and decrypted after receiving
- SSL certificates help with encryption (HTTPS)
- Encryption in flight ensures no MITM (man in the middle attack) can happen

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/38e6a562-27fe-4d96-8c18-ef8962b30ce3/Untitled.png)

### Server side encryption at rest

- Data is encrypted after being received by the server
- Data is decrypted before being sent
- It is stored in an encrypted form thanks to a key (usually a data key)
- The encryption / decryption keys must be managed somewhere and the server must ahve access to it

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a8936a21-41ab-4c12-ba87-bd835e0febf8/Untitled.png)

### Client side encryption

- Data is encrypted by the client and never decrypted by the server
- Data will be decrypted by a receiving client
- The server should not be able to decrypt the data
- Could leverage Envelope Encryption

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2536891b-8301-4284-860d-d564a7089d07/Untitled.png)

## AWS KMS (Key Management Service)

- Anytime you hear “encryption” for an AWS service, it’s most likely KMS
- AWS manages encryption keys for us
- Fully integrated with IAM for authorization
- Easy to control access to your data
- Able to audit KMS Key usage using CloudTrail
- Seamlessly integrated into most AWS service (EBS, S3, RDS, SSM…)
- **Never ever store your secrets in plaintext, esspecially in your code**
  - KMS Key encryption also available through API calls (SDK, CLI)
  - Encrypted secrets can be stored in the code / environment variables

### KMS Keys Types

- **KMS Keys is the new name of KMS Customer Master Key**
- **Symmetric (AES-256 keys)**
  - Single encryption key that is used to Encrypt and Decrypt
  - AWS service that are integrated with KMS use Symmetric CMKs
  - You never get access to the KMS Key unencrypted (must call KMS API to use)
- **Asymmetric (RSA & ECC key pairs)**
  - Public (Encrypt) and Private Key (Decrypt) pair
  - Used for Encrypt/Decrypt, or Sign/Verify operations
  - The public key is downloadable, but you can’t access the Private Key unencrypted
  - Use case: encryption outside of AWS users who can’t call the KMS API

### AWS KMS (Key Management Service)

- Types of KMS Keys:
  - AWS Owned Keys (free): SSE-S3, SSE-SQS, SSE-DDB (default key)
  - AWS Managed Key: **free** (aws/service-name, example: aws/rds or aws/ebs)
  - Customer managed keys created in KMS: **$1 / month**
  - Customer managed keys imported (must be symmetric key): $1 / month
  - - pay for API call to KMS ($0.03 / 10000 calls)
- Automatic Key rotation:
  - AWS-managed KMS Key automatic every 1 year
  - Customer-managed KMS Key: (must be enabled) automatic every 1 year
  - Imported KMS Key: only manual rotation possible using alias

### Copying Snashots across regions

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1d2cf18e-73cd-42ad-95d7-e746728d823d/Untitled.png)

### KMS Key Policies

- Control access to KMS keys, “similar” to S3 bucket policies
- Diffirence: you cannot control access without them

- **Default KMS Key Policy:**
  - Created if you don’t provide a specific KSM Key Policy
  - Complete access to the key to the root user = entire AWS account
- **Custom KMS Key Policy:**
  - Define users, roles that can access the KMS key
  - Define who can administer the key
  - Useful for cross-account access of your KMS Key

### Coying Snapshots across accounts

1. Create a Snapshot, encrypted with your own KMS Key (Customer Managed Key)
2. **Attach a KMS Key Policy to authorize across-account access**
3. Share the encrypted snapshot
4. (in target) Create a copy of the Snapshot, encrypt it with a CMK in your account
5. Create a volume from the snapshot

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/91330eae-043a-4ea5-82b7-71a1011b8073/Untitled.png)

### KMS Multi-Region Keys

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5ccc2a3-0442-401a-b3cf-f8223e0af028/Untitled.png)

- Identical KMS keys in different AWS Regions that can be used interchangeably
- Multi-Region keys have the same key ID, key material, automatic rotation…
- Encrypt in one Region and decrypt in other Regions
- No need to re-encrypt or making cross-Region API calls
- KMS Multi-Region are NOT global (Primary + Replicas)
- Each Multi-Region key is managed **independently**
- Use casses: global client-side encryption, encryption on Global DynamoDB, Global Aurora

### Dynamo Global Tables and KMS Multi-Region Keys Client-Side encryption

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/18a171f8-a96d-4c24-92a9-39cefca6c145/Untitled.png)

- We can encrypt specific attributes client-side in our DynamoDB table using the **Amazon DynamoDB Encryption Client**
- Combined with Global Tables, the client-side encrypted data is replicated to other regions
- If we use a multi-region key, replicated in the same region as the DynamoDB Global table, then clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side
- Using client-side encryption we can protect specific fields and guarantee only decryption if the client has access to an API key

### Global Aurora and KMS Multi-Region Keys Client-Side encryption

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/696bc2e8-b9a7-4fb4-94ec-3d4af62119d8/Untitled.png)

- We can encrypt specific attributes client-side in our Aurora table using the **AWS Encryption SDK**
- Combined with Aurora Global Tables, the client-side encrypted data is replicated to other regions
- If we use a multi-region key, replicated in the same regions as the Global Aurora DB, then clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side

### S3 Replication Encryption Considerations

- **Unencrypted object and object encrypted with SSE-S3 are replicated by default**
- Objects encrypted with SSE-C (customter provided key) are never replicated
- **For objects encrypted with SSE-KMS**, you need to enable the option
  - Specify which KMS Kye to encrypt the objects within the target bucket
  - Adapt the KMS Key Policy for the target key
  - An IAM Role with KMS: Decrypt for the source KMS Key KMS: Encrypt for the target KMS Key
  - You might get KMS throtting errors, in which case you can ask for a Service Quotas increase
- **You can use mutli-region AWS KMS Keys, but they are currently treated as independent keys by Amazon S3 (the object will still be decrypted and then encrypted)**

### AMI Sharing Process Encrypted via KMS

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e644bdfa-7e0e-428c-abff-73757e3e1b0c/Untitled.png)

1. AMI in Source Account is encrypted with KMS Key form Source Account
2. Must modify the image attribute to add a **Launch Permission** which corresponds to the specified target AWS account
3. Must share the KMS Keys used to encrypted the snapshot the AMI references with the target account / IAM Role
4. The IAM Role/User in the target account must have the permissions to DescribeKey, ReEncrypted, CreateGrant, Decrypt
5. When launching an EC2 instance from the AMI, optionally the target account can specify a new KMS key in its own account to re-encrypt the volumes

## SSM Parameter Store

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6e65a9f2-5b1e-4334-9dc0-2c8523504c1a/Untitled.png)

- Secure storage for configuration and secrets
- Optional Seamless Encryption using KMS
- Serverless, scalable, durable, easy SDK
- Version tracking of configurations / secrets
- Security through IAM
- Notifications with Amazon EventBridge
- Integration with CloudFormation

### SSM Parameter Store Hierarchy

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1db1857a-913b-4729-b5d4-a54c5766877b/Untitled.png)

### Standard and advanced parameter tiers

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ec3da04d-6a2f-46a7-93e7-f727f1aca30f/Untitled.png)

### Parameters Policies (for advanced parameters)

- Allow to assign a TTL to a parameter (expiration data) to force updating or deleting sensitive data such as passwords
- Can assign multiple policies at a time

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/39273c5e-996b-4c00-a283-8e96119a01c5/Untitled.png)

## AWS Secrets Manager

- Newer service, meant for storing secrets
- Capability to force **rotation of secrets** every X days
- Automate generation of secrets on rotation (uses Lambda)
- Integration with **Amazon RDS** (MySQL, PostgreSQL, Aurora)
- Secrets are encrypted using KMS
- Mostly meant for RDS integration

### Multi-Region Secrets

- Replicate Secrets across multiple AWS Regions
- Secrets Manager keeps read replicas in sync with the primary Secret
- Ability to promote a read replica Secret to a standalone Secret
- Use cases: multi-region apps, disaster recovery strategies, multi-region DB…

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a904b718-87f3-4c86-9dd6-51aff6580890/Untitled.png)

## AWS Certificate Manager (ACM)

- Easily privosion, manage, and deploy **TLS Certificates**
- Provide in-flight encryption for websites (HTTPS)
- Supports both public and private TLS certificates
- Free of charge for public TLS certificates
- Automatic TLS certificates renewal
- Integrations with (load TLS certificates on)
  - Elastic Load Balancers( CLB, ALB, NLB)
  - CloudFront Distributions
  - APIs on API Gateway

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4bc4576d-7813-40b4-ab97-3d84e5a957ee/Untitled.png)

- Cannot use ACM with EC2 (can’t be extracted)

### Requesting Public Certificates

1. List domain names to be included in the certificate
   1. Fully Qualified Domain Name (FQDN): corp.example.com
   2. Wildcard Domain: \*.example.com
2. Select Validation Method: DNS Validation or Email validation
   1. DNS Validation is preferred for automation purposes
   2. Email validation will send emails to contact address in the WHOIS database
   3. DNS Validation will leverage a CNAME record to DNS config (ex: Route 53)
3. It will take a few hours to get verified
4. The Public Certificate will be enrolled for automatic renewal
   - ACM automatically renews ACM-generated certificates 60 days before expiry

### Importing Public Certificates

- Option to generate the certificate outside of ACM and then import it
- **No automatic renewal**, must import a new certificate before expiry
- **ACM sends daily expiration events** starting 45 days prior to expiration
  - The # of days can be configured
  - Events are apprearing in EventBridge
- **AWS Config** has a managed rule named _acm-certificate-expiration-check_ to check for expiring certificates (configurable number of days)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/92239f17-181b-445f-a889-1d6c286c9673/Untitled.png)

### Integration with ALB

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1aedc32e-96d0-476e-9ea9-08b2c61f8d27/Untitled.png)

### Endpoint Types

- **Edge-Optimized (default)**: For global clients
  - Requests are routed through the CloudFront Edge locations (improves latency)
  - The API Gateway still lives in only one region
- **Regional**:
  - For clients within the same region
  - Coudl manually combine with CloudFront (more control over the caching strategies and the distribution)
- **Private**:
  - Can only be accessed from your VPC using an interface VPC endpoint (ENI)
  - Use a resource policy to define access

### Integration with API Gateway

- Create a **Custom Domain Name** in API Gateway
- **Edge-Optimized (default)**: For global clients
  - Requests are routed through the CloudFront Edge locations (improves latency)
  - The API Gateway still lives in only one region
  - **The TLS Certificate must be in the same region as CloudFront, in us-east-1**
  - Then setup CANAME or (better) A-Alias record in Route 53
- Regional:
  - For clients within the same region
  - **The TLS Cerificate must be imported on API gateway, in the same region as the API Stage**
  - Then setup CNAME or (better) A-Alias record in Route 53

## AWS WAF - Web Application Firewall

- Protects your web applications from common web exploits (Layer 7)
- **Layer 7 is HTTP** (vs Layer 4 is TCP/UDP)
- Deploy on
  - **Application Load Balancer**
  - **API Gateway**
  - **CloudFront**
  - **AppSync GraphQL API**
  - **Cognito User Pool**
- Define Web ACL (Web Access Control List) Rules:
  - **IP Set: up to 10,000 IP Addresses** - use multiple Rules for more IPs
  - HTTP headers, HTTP body, or URI strings Protects from common attack - **SQL injection** and **Cross-Site-Scripting (XSS)**
  - Size constraints, **geo-match (block countries)**
  - **Rate-based rules** (to count occurrences of events) - **for DDoS protection**
- Web ACL are Regional except for CloudFront
- A rule group is a **reusable set of rules that you can add to a web ACL**

### Fixed IP while using WAF with a Load Balancer

- WAF does not support the Network Load Balancer (Layer 4)
- We can use Global Accelerator for fixed IP and WAP on the ALB

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1517b163-49a4-4449-b8e9-b8f654c89b0d/Untitled.png)

## AWS Shield - Protect from DDoS attack

- **DDoS**: Distributed Denial of Service - many requests at the same time
- **AWS Shield Standard**:
  - Free service that is activated for every AWS customer
  - Provides protection from attacks such as SYN/UDP Floods, Reflection attacks and other layer 3/layer 4 attacks
- **AWS Shield Advanced**:
  - Optional DDoS mitigation service ($3,000 per month per organization)
  - Protect against more sophisticated attack on EC2, Elastic Load Balancing, CloudFront, Global Accelerator, Route 53
  - 24/7 acces to AWS DDoS response team (DRP)
  - Protect against higher fees during usage spikes due to DDoS
  - Shield Advanced automatic application layer DDoS mitigation automatically creates, evaluates and deploys AWS WAF rules to mitigate layer 7 attacks
## AWS Firewall Manager

- **Manage rules in all accounts of an AWS Organization**
- Security policy: common set of security rules
    - WAF rules (Application Load Balancer, API Gateways, CloudFront)
    - AWS Shield Advanced (ALB, CLB, NLB, Elastic IP, CloudFront)
    - Security Groups for EC2, Application Load Balancer and ENI resources in VPC
    - AWS Network Firewall (VPC Level)
    - Amazon Route 53 Resolver DNS Firewall
    - Policies are created at the region level
- **Rules are applied to new resources as they are created (good for compliance) across all and future accounts in your Organization**

### WAF vs Firewall Manager vs Shield

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/31fa0e49-7f76-44f1-88e5-ae8a515a6ccb/Untitled.png)

- **WAF, Shield and Firewall Manager are used together for comprehensive protection**
- Define your Web ACL rules in WAF
- For granular protection of your resources, WAF alone is the correct choice
- If you want to use AWS WAF across accounts, accelerate WAF configuration, automate the protection of new resources, use Fire Manager with AWS WAF
- Shield Advanced adds additional features on top of AWS WAF, such as dedicated support from the Shield Response Team (SRT) and advanced reporting
- If you’re prone to frequent DDoS attacks, consider purchasing Shield Advanced