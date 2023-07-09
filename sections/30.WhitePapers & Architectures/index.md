# Overview

- Well Architected Framework Whitepaper
- Well Architected Tool
- AWS Trusted Advisor
- Reference architectures resources (for real-world)
- Disaster Recovery on AWS Whitepaper

## Well Architected Framework General Guiding Principles

- https://aws.amazon.com/architecture/well-architected/
- Stop guessing your capacity needs
- Test systems at production scale
- Automate to make architectural experimentation easier
- Allow for evolutionary architectures
    - Design based on changing requirements
- Drive architectures using data
- Improve through game days
    - Simulate applications for flash sale days

## Well Architected Framework 6 Pillars

- **Operational Excellence**
- **Security**
- **Reliability**
- **Performance Efficiency**
- **Cost Optimization**
- **Sustainability**

- **They are not something to balance, or trade-offs, they’re a synergy**

## AWS Well-Architected Tool

- Free tool to review your architectures against the 6 pillars Well-Architected Framework and adopt architectural best practices
- How does it work?
    - Select your workload and answer questions
    - Review your answers against the 6 pillars
    - Obtain advice: get videos and documentations, generate a report, see the results in a dashboard
- Look:  https://console.aws.amazon.com/wellarchitected

## Trusted Advisor

- No need to install anything - high level AWS account assessment
- Analyze your AWS accounts and provides recommendation on 5 categories (**HỌC THUỘC CÓ TRONG EXAM)**
- **Cost optimization**
- **Performance**
- **Security**
- **Fault tolerance**
- **Service limits**

### Trusted Advisor - Support Plans

**7 CORE CHECKS**

**Basic** & **Developer** Support plan

- S3 Bucket permissions
- Security Groups - Specific Ports Unrestricted
- IAM Use (one IAM user minimum)
- MFA on Root Account
- EBS Public Snapshots
- RDS Public Snapshots
- Service Limits

FULL CHECKS

**Business** & **Enterprise** Support Plan

- Full Checks available on the 5 categories
- Ability to set CloudWatch alarms when reaching limits
- **Programmatic Access using AWS Support API**

## More Architecture Examples

- We’ve explored the most important architectural patterns:
    - Classic: EC2, ELB, RDS, ElastiCache, etc..
    - Serverless: S3, Lambda, DynamoDB, CloudFront, API Gateway…
- More examples:
    - https://aws.amazon.com/architecture
    - https://aws.amazon.com/solutions