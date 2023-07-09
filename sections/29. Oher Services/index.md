# Overview

## What is CloudFormation

- CloudFormation is a declarative way of outlining your AWS Infrastructure, for any resources (most of them are supported)
- For example, within a CloudFormation template, you say:
    - I want a security group
    - I want two EC2 instances using this security group
    - I want an S3 bucket
    - I want a load balancer (ELB) in front of these machines
- Then CloudFormation creates those for you, in the **right order**, with the **exact configuration** that you specify

### Benefits of AWS CloudFormation

- **Infrastructure as code**
    - No resources are manually created, which is excellent for control
    - Changes to the infrastructure are reviewed through code
- **Cost**
    - Each resources within the stack is tagged with an identifier so you can easily see how much a stack costs you
    - You can estimate the costs of your resources using the CloudFormation template
    - Saving strategy: In Dev, you could automation deletion of templates at 5 PM and recreated at 8AM, safely
- Productivity
    - Ability to destroy and re-create an infrastructure on the cloud in the fly
    - Automated generation of Diagram for your templates
    - Declarative programming (no need to figure out ordering and orchestration)
- Don’t re-invent the wheel
    - Leverage existing templates on the web
    - Leverage the documentation
- **Supports (almost) all AWS resources:**
    - Everything we’ll see in this course is supported
    - You can use “custom resources” for resources that are not supported

### CloudFormation Stack Designer

- Example: WordPress CloudFormation Stack
- We can see all the resources
- We can see the relations between the components

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f4f1f9b-f0db-4731-b6f9-fb1dbf497581/Untitled.png)

## Amazon Simple Email Service (Amazon SES)

- Fully managed service to send emails securely, globally and at scale
- Allows inbound/outbound emails
- Reputation dashboard, performance insights, anti-spam feedback
- Provides statistics such as email deliveries, bounces, feedback loop results, email open
- Supports DomainKeys Identified Mail (DKIM) and Sender Policy Framework (SPF)
- Flexible IP deployment: shared, dedicated, and customer-owned IPs
- Send email using your application using AWS Console, APIs, or SMTP
- Use cases: transactional, marketing and bulk email communications

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70751f6a-2ee9-47b3-bd10-0f23b38e5492/Untitled.png)

## Amazon Pinpoint

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/623606e9-e2a6-4c51-b5e4-83fc7cda3c56/Untitled.png)

- Scalable 2-way (outbound/inbound) marketing communications service
- Supports email, SMS, push, voice, and in-app messaging
- Ability to segment and personalize messages with the right content to customers
- Possibility to receive replies
- Scales to billions of messages per day
- Use cases: run campaigns by sending marketing, bulk, transactional SMS messages
- Versus Amazon SNS or Amazon SES
    - In SNS & SES you managed each message’s audience, content, and delivery schedule
    - In Amazon Pinpoint, you create message templates, delivery schedules, highly-targeted segments, and full campaigns

## System Manager - SSM Session Manager

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/372602b1-8010-409d-9f87-f868739f3337/Untitled.png)

- Allows you to start a secure shell on your EC2 and on-premises servers
- No SSH access, bastion hosts, or SSH keys needed
- No port 22 needed (better security)
- Supports Linux, macOS, and Windows
- Send session log data to S3 or CloudWatch Logs

## Systems Manager - Run Command

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c8b60452-0bf3-4766-8e84-de53d12fa05a/Untitled.png)

- Execute a document (=script) or just run a command
- Run command across multiple instances (using resource groups)
- No need for SSH
- Command Output can be shown in the AWS Console, sent to S3 bucket or CloudWatch Logs
- Send notifications to SNS about command status (in progress, success, failed…)
- Integrated with IAM & CloudTrail
- Can be invoke using EventBridge

## Systems Manager - Patch Manager

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cc321619-aaee-414b-8f2b-e643ab4021e5/Untitled.png)

- Automates the process of patching managed instances
- OS updates, applications updates, security updates
- Supports EC2 instances and on-premises servers
- Supports Linux, macOS and Windows
- Patch on-demand or on a schedule using **Maintenance Windows**
- Scan instances and generate patch compliance report (missing patches)

## Systems Manager - Maintenance Windows

- Defines a schedule for when to perform actions on your instances
- Example: OS patching, updating drivers, installing software…
- Maintenance Window contains
    - Schedule
    - Duration
    - Set of registered instances
    - Set of registered tasks

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0aebb428-065e-4b00-baef-0021edd68ec4/Untitled.png)


## Systems Manager - Automation

- Simplifies common maintenance and deployment tasks of EC2 instances and other AWS resources
- Examples: restart instances, create an AMI, EBS snapshot
- **Automation Runbook** - SSM Documents to define actions preformed on your EC2 instances or AWS resources (pre-defined or custom)
- Can be triggered using:
    - Manually using AWS Console, AWS CLI or SDK
    - Amazon EventBridge
    - On a schedule using Maintenance Windows
    - By AWS Config for rules remediations

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ed8697d4-65dc-43e6-a7e1-0236fa8ab345/Untitled.png)

## Amazon Elastic Transcoder

- Elastic Transcoder is used to **convert media files stored in S3 into media files in the formats required by consumer playback devices (phones etc…)**
- Benefits:
    - Easy to use
    - Highly scalable - can handle large volumes of media files and large file sizes
    - Cost effective - duration-based pricing model
    - Fully managed & secure, pay for what you use

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a56be449-542c-4d40-85f7-d3e66f9af245/Untitled.png)

    ## AWS Batch

- Fully managed batch processing at **any scale**
- Efficiently run 100,000s of computing batch jobs on AWS
- A “batch” job is a job with a start and an end (opposed to continuous)
- Batch will dynamically launch **EC2 instances** or **Spot Instances**
- AWS Batch provisions the right amount of compute / memory
- You submit or schedule batch jobs and AWS Batch does the rest!
- Batch jobs are defined as **Docker Images** and **run on ECS**
- Helpful for cost optimizations and focusing less on the infrastructure

### Simplified Example

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/968b255f-d348-4905-b605-7ed2918871d6/Untitled.png)

### Batch vs Lambda

- Lambda:
    - Time limit
    - Limited runtimes
    - Limited temporary disk space
    - Serverless
- Batch:
    - No time limit
    - Any runtime as long as it’s packaged as a Docker image
    - Rely on EBS / instance store for disk space
    - Replies on EC2 (can be managed by AWS)


## Amazon AppFlow

- Fully managed integration service that enables you to securely transfer data between **Software-as-a-Service (SasS) applications and AWS**
- Sources: **Salesforce**, SAP, Zendesk, Slack, and ServiceNow
- Destinations: AWS services like **Amazon S3, Amazon Redshift** or non-AWS such as SnowFlak and Salesforce
- Frequency: on a schedule, in response to events, or on demand
- Data transformation capabilities like filtering and validation
- Encrypted over the public internet or privately over AWS PrivateLink
- Don’t spend time writing the integrations and leverage APIs immediately