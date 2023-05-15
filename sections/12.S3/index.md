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

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3620ab2c-c474-46f2-8811-73fc4e8de8bb/Untitled.png)

- **These settings were created to prevent company data leaks**
- If you know your bucket should never be public, leave these on
- Can be set at the account level