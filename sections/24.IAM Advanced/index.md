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