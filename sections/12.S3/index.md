# Overview

## Introduction

- Amazon S3 is one of the main building blocks of AWS
- It’s advertised as ‘infinitely scaling’ storage
- Many websites use Amazon S3 as a backbone
- Many AWS services use Amazon S3 as an integration as well
- We’ll have a step-by-step approach to S3

### Amazon S3 Use cases

- Backup and storage
- Disaster Recovery
- Archive
- Hybrid Cloud storage
- Application hosting
- Media hosting
- Data lakes & big data analytics
- Software delivery
- Static website

### Amazon S3 - Buckets

- Amazon S3 allows people to store object (files) in “buckets” (directories)
- Buckets must have a **globally unique name (across all regions all accounts)**
- Buckets are defined at the region level
- **S3 is not global service, it’s created in a region**
- Naming convention
    - No uppercase, no underscore
    - 3-63 characters long
    - Not an IP
    - Must start with lowercase letter or number
    - Must **NOT** start with the prefix **xn—**
    - Must **NOT** end with the suffic **-s3alias**

### Amazon S3 - Objects

- Objects (files) have a Key
- The **key** is the FULL path:
    - s3://my-bucket/**my_file.txt**
    - s3://my-bucket/**my_folder/another_folder/my_file.txt**
- The key is composed of **prefix** + **object name**
    - s3://my-bucket/**my_folder/another_folder/my_file.txt**
- There’s no concept of ‘directories’ within buckets (although the UI will trick you to think otherwise)
- Just keys with very long names that contain slashes (’/’)
- Object values are the content of the body:
    - Max Object Size is 5TB (5000GB)
    - If uploading more than 5GB, must use ‘multi-part upload’
- Metadata (list of text key / value pairs - system or user metadata)
- Tags (Unicode key / value pair - up to 10) - useful for security / lifecycle
- Version ID (if versioning is enabled)
## Amazon S3 - Security

- **User-Based**
    - **IAM Policies** - which API calls should be allowed for a specific user from IAM
- **Resource-Based**
    - **Bucket Policies** - bucket wide rules from the S3 console - allows cross account
    - **Object Access Control List (ACL)** - finer grain (can be disabled)
    - **Bucket Access Control List (ACL)** - less common (can be disabled)
- **Note**: an IAM principal can access an S3 object if:
    - The user IAM permissions ALLOW it OR the resource policy ALLOWS it
    - AND there’s no explicit DENY
- **Encryption**: encrypt objects in Amazon S3 using encryption keys

### S3 Bucket Policies

- JSON based policies
    - Resources: buckets and objects
    - Effect: Allow / Deny
    - Actions: Set of API to Allow or Deny
    - Principal: The account or user to apple the policy to
- Use S3 bucket for policy to:
    - Grant public access to the bucket
    - Force object to be encrypted at upload
    - Grant access to another account (Cross Account)

### Bucket settings for Block Public Access


- **These settings were created to prevent company data leaks**
- If you know your bucket should never be public, leave these on
- Can be set at the account level

## Amazon S3 - Static Website Hosting

### Introduction

- S3 can host static websites and have them accessible on the Internet
- The website URL will be (depending on the region)
    - http://bucket-name.s3-website-aws-region.amazonaws.com
- If you get a **403 Forbidden** error, make sure the bucket policy allows public reads

## Amazon S3 - Versioning

### Introduction

- You can version your files in Amazon S3
- It is enabled at the bucket level
- Same key overwrite will change the ‘version’: 1,2,3…
- It is best practice to version your buckets
    - Protect against unintended deletes (ability to restore a version)
    - Easy roll back to previous version
- Notes:
    - Any files that is not versioned prior to enabling versioning will have version ‘null’
    - Suspending versioning does not delete the previous versions

## Amazon S3 - Replication (CRR & SRR)

- **Must enable Versioning** in source and destination buckets
- **Cross-region Replication (CRR)**
- **Same-region Replication (SRR)**
- Buckets can be in different AWS accounts
- Copying is asynchronous
- Must give proper IAM permissions to S3
- Use cases:
    - **CRR** - compliance, lower latency access, replication across accounts
    - **SRR** - log aggregation, live replication between production and test accounts
- After you enable Replication, only new objects are replicated
- Optionally, you can replicate existing objects using **S3 Batch Replication**
    - Replicates existing objects and objects that failed replication
- For DELETE operations
    - Can replicate delete markers from source to target (optional setting)
    - Deletions with a version ID are not replicated (to avoid malicious deletes)
- **There is no ‘chaining’ of replication**
    - If bucket i has replication into bucket 2, which has replication into bucket 3
    - Then objects created in buck 1 are not replicated to bucket 3

## S3 Storage Classes

### Introduction

- Amazon S3 Standard - General Purpose
- Amazon S3 Standard - Infrequent Access (IA)
- Amazon S3 One Zone - Infrequent Access
- Amazon S3 Glacier Instant Retrieval
- Amazon S3 Glacier Flexible Retrieval
- Amazon S3 Glacier Deep Archive
- Amazon S3 Intelligent Tiering

- Can move between classes manually or using S3 Lifecycle configurations

### S3 Durability and Availability

- Durability:
    - High durability (99,999999999%, 11 9’s) of objects across multiple AZ
    - If you store 10,000,000 objects with Amazon S3, you can on average expect to incur a loss of a single object once every 10,0000 years
    - Same for all storage classes
- Availability
    - Measures how readily available a service is
    - Varies depending on storage class
    - Example: S3 standard has 99,99% availability = not available 53 minutes a year

### S3 Standard - General Purpose

- 99,99% Availability
- Used for frequently accessed data
- Low latency and high throughput
- Sustain 2 concurrent facility failures
- Use case: Big Data analytics, mobile & gaming applications, content distribution…

### S3 Storage Classes - Infrequent Access

- For data that is less frequently accessed, but requires rapid access when needed
- Lower cost than S3 standard
- **Amazon S3 Standard-Infrequent Access (S3 Standard-IA)**
    - 99,99% Availability
    - Use cases: Disaster Recovery, backups
- **Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)**
    - High durability (99,9999999999%) in a single AZ, data lost when AZ is destroyed
    - 99,5% Availability
    - Use cases: Storing secondary backup copies of on-premise data, or data you can recreate

### S3 Glacier Storage Classes

- Low-cost object storage meant for archiving / backup
- Pricing: price for storage + object retrieval cost
- **Amazon S3 Glacier Instant Retrieval**
    - Millisecond retrieval, great for data accessed once a quarter
    - Minimum storage duration of 90 days
- **Amazon S3 Glacier Flexible Retrieval (formerly Amazon S3 Glacier)**:
    - Expedited ( 1 to 5 minutes), Standard ( 3 to 5 hours), Bulk ( 5 to 12 hours) - free
    - Minimum storage duration of 90 days
- **Amazon S3 Glacier Deep Archive - for long term storage:**
    - Standard (12 hours), Bulk (48 hours)
    - Minimum storage duration of 180 days

### S3 Intelligent-Tiering

- Small monthly monitoring and auto-tiering fee
- Move object automatically between Access Tier based on usage
- There are no retrieval charges in S3 Intelligent-Tiering

- *Frequent Access tier (automatic)*: default tier
- *Infrequent Access tier (automatic)*: object not accessed for 30 days
- *Archive Instant Access tier (automatic)*: objects not accessed for 90 days
- *Archive Access tier (optional)*: configurable from 90 days to 700+ days
- *Deep Archive Access tier (optional)*: config from 180 days to 700+ days

## Advanced Amazon S3

### Moving between Storage Classes

- You can transition objects between storage classes
- For infrequently accessed object, move them to **Standard IA**
- For archive objects that you don’t need fast access to, move them to Glacier or Glacier Deep Archive
- Moving object can be automated using a Lifecycle Rules

### Lifecycle Rules

- **Transition Actions** - configure objects to transition to another storage class
    - Move objects to Standard IA class 60 days after creation
    - Move to Glacier for archiving after 6 months
- **Expiration actions** - configure objects to expire (delete) after some time
    - Access logs files can be set to delete after a 365 days
    - **Can be used to delete old versions of file (if versioning is enabled)**
    - Can be used to delete incomplete Multi-part uploads
- Rules can be created for a certain prefix (example: s3://mybucket/mp3/*)
- Rules can be created for certain objects Tags (example:Deparment:Finance)