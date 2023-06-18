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