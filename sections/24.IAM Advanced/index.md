# Overview

## AWS Organizations

- Global service
- Allows to manage multiple AWS accounts
- The main account is the management account
- Other accounts are member accounts
- Member accounts can only be part of one organization
- Consolidated Billing across all accounts - single payment method
- Pricing benefits from aggregated usage (volume discount for EC2, S3…)
- Shared reserved instances and Savings Plans discounts across accounts
- API is available to automate AWS account creation

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d7d02115-9ea7-4e78-a29a-1e99bfa71023/Untitled.png)

- **Advantages**
  - Multi Account vs One Account Multi VPC
  - Use tagging standards for billing purposes
  - Enable CloudTrail on all accounts, send logs to central S3 account
  - Send CloudWatch Logs to central logging account
  - Establish Cross Account Roles for Admin purposes
- **Security: Service Control Policies (SCP)**

  - IAM policies applied to OU or Accounts to restrict Users and Roles
  - They do not apply to the management account (full admin power)
  - Must have an explicit allow (does not allow anything by default - like IAM)

  ### SCP Hierarchy

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e1aa7fa7-21f8-428b-b263-6f1938bf5b69/Untitled.png)

  ### SCP Examples Blocklist and Allowlist strategies

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/700d43e2-99ac-4388-b949-91980d05cb40/Untitled.png)

## IAM Conditions

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84e5b1b2-6235-4e4e-9bc1-07477d85e89b/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/15ed5f9f-bf6d-45a7-9b6c-842926b9bb58/Untitled.png)

### IAM for S3

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1e13a4dc-77a1-4c0f-bb4e-0729e06a64d1/Untitled.png)

### Resource Policies & aws:PrincipalOrgID

- **aws:PrincipalOrgID** can be used in any resources policies to restrict access to accounts that are member of an AWS Organization

### IAM Roles vs Resource Based Policies

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/157875b9-a48d-46d7-ae1b-6104186710e5/Untitled.png)

- Cross account:
  - Attaching a resource-based policy to a resource (example: S3 bucket policy)
  - OR using a role as a proxy
- **When you assume a role (user, application or service), you give up your original permissions and take the permissions assigned to the role**
- When using a resource-base policy, the principal doesn’t have to give up his permissions
- Example: User in account A needs to scan a DynamoDB table in Account a and dump it in an S3 bucket in Account B
- Supported by: Amazon S3 buckets, SNS topics, SQS queues, etc..

### Amazon EventBridge - Security

- When a rule runs, it needs permissions on the target
- **Resource-based policy: Lambda, SNS, SQS, CloudWatch Logs, API Gateway…**
- I**AM role: Kinesis stream, System Manager Run Command, ECS task**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f61cbb22-1651-40db-b24a-ac8e8bfb7b47/Untitled.png)

### IAM Permission Boundaries

- IAM Permission Boundaries are supported for users and roles (not groups)
- Advanced feature to use a managed policy to set the maximum permissions an IAM entity can get

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4a451f7d-536b-4eff-94c3-6650d679c599/Untitled.png)

- Can be used in combinations of AWS Organizations SCP

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/44bc4580-9b6a-4274-ae81-11eddd79a127/Untitled.png)

- Use cases:
  - Delegate responsibilities to non administrators within their permission boundaries, for example create new IAM users.
  - Allow developers to self-assign policies and manage their own permissions, while make sure they can’t “escalate” their privileges (= make themselves admin)
  - Useful to restrict one specific user (instead of a whole account using Organizations & SCP)

### IAM Policy Evaluation Logic

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/90393f8a-fb88-4933-a48c-4e191b8c06cf/Untitled.png)

### Example IAM Policy

- Can you perform sqs:CreateQueue? ⇒ No ⇒ Because explicit Deny
- Can you perform sqs:DeleteQueue? ⇒ No ⇒ Because explicit Deny
- Can you perform ec2:DescribeInstances? ⇒ No ⇒ Because no explicit Deny also no explicit Allow

## AWS IAM Identity Center (successor to AWS SSO)

- One login (single sign-on) for all your
  - **AWS accounts in AWS Organizations**
  - Business cloud applications (e.g: Saleforce, Box, Microsoft 365…)
  - SAML2.0-enabled applications
  - EC2 Window Instances
- Identity providers
  - Built-in identity store in IAM Identity Center
  - 3rd party: Active Directory (AD), OneLogin, Okta…

### AWS IAM Identity Center - Login Flow

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2b8f560f-7166-4564-acc7-a15d0da32d34/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c4cc25e7-9fca-4d18-b8c4-0037910906e8/Untitled.png)

### IAM Identity Center

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/475d8f86-d249-4aa0-bf2a-a54a926f8411/Untitled.png)

### AWS IAM Identity Center Fine-grained Permissions and Assignments

- **Multi-Account Permissions**
  - Manage access across AWS accounts in your AWS Organization
  - Permission Sets - a collection of one or more IAM Policies assigned to users and groups to define AWS access
- **Application Assignments**
  - SSO access to many SAML 2.0 business applications (Salesforce, Box, MS 365…)
  - Provide required URLS, certificates, and metadata
- **Attribute-Based Access Control (ABAC)**
  - Fine-grained permissions based on users’s atttributes stored in IAM Identity Center Identity Store
  - Example: cost center, title, locale…
  - Use case: Define permissions once, then modify AWS access by changing the attributes

## AWS Microsoft Active Directory

- Found on any Windows Server with AD Domain Services
- Database of **objects**: User Account, Computers , Printers, File Shares, Security Groups
- Centralized security management, create account, assign permissions
- Object are organized in **trees**
- A group of trees is a **forest**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6e4dfce3-8815-4cd1-ab3a-1aea079b2419/Untitled.png)

### AWS Directory Services

- **AWS Managed Microsoft AD**
  - Create your own AD in AWS, manage users locally, supports MFA
  - Establish “trust” connections with your on-premises AD
- **AD Connector**
  - Directory Gateway (proxy) to redirect to on-premises AD, support MFA
  - Users are managed on the on-premises AD
- **Simple AD**

  - AD-compatible managed directory on AWS
  - Cannot be joined with on-premises AD

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f9bce00-6f21-42f2-8435-963584a65a58/Untitled.png)

### IAM Identity Center - Active Directory Setup

- Connect to an AWS Managed Microsoft AD (Directory Service)

  - Intergration is out of the box

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c4aec30-ae7a-4dd9-bed7-2e8898cc6d06/Untitled.png)

- Connect to a Self-Managed Directory
  - Create Two-way Trust Relationship using AWS Managed Microsoft AD
  - Create an AD Connector
