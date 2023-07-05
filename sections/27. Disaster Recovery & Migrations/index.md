# Overview

## Disaster Recovery

- Any event that has a negative impact on a company’s business continuity or finances is a disaster
- Disaster recovery (DR) is about preparing for and recovering from a disaster
- What kind of disaster recovery?
  - On-premise ⇒ On-premise: traditional DR, and very expensive
  - On-premise ⇒ AWS Cloud: hybrid recovery
  - AWS Cloud Region A ⇒ AWS Cloud Region B
- Need to define two terms:

  - **RPO: Recovery Point Objective**
  - **RTO: Recovery Time Objective**

  ### RPO vs RTO

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/880c769b-d1ab-45d5-8a48-ee5b5685ecfe/Untitled.png)

### Disaster Recovery Strategies

- Backup and Restore
- Pilot Light
- Warm Standby
- Hot Site / Multi Site Approach

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/21f3b5a5-fef8-430e-9fb5-2d38769fd7c8/Untitled.png)

### Backup and Restore (High RPO)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/74e8e3d5-e911-4c34-878f-c94755d6ef2c/Untitled.png)

### Disaster Recovery - Pilot Light

- A small version of the app is always running in the cloud
- Useful for the critical core (pilot light)
- Very similar to Backup and Restore
- Faster than Backup and Restore as critical systems are already up

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/329e0cec-66a7-45e4-a085-4fc05ab9ff97/Untitled.png)

### Warm Standby

- Fully system is up and running, but at minimum size
- Upon disaster, we can scale to production load

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e4839172-2718-4202-8914-c2dc6023654a/Untitled.png)

### Multi Site / Hot Site Approach

- Very low RTO (minutes or seconds) - very expensive
- Full Production Scali is running AWS and on Premise

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/21c67b6f-3e5d-45e1-97bf-5e29c1633d76/Untitled.png)

### All AWS Multi Region

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c55e16ef-74a1-47ba-8003-107925ac45a6/Untitled.png)

### Disaster Recovery Tips

- Backup
  - EBS Snapshots, RDS automated backups / Snapshots…etc…
  - Regular pushes to S3 / S3 IA / Glacier, Lifecycle Policy, Cross Region Replication
  - From On-Premise: Snowball or Storage Gateway
- High Availability
  - Use Route53 to migrate DNS over from Region to Region
  - RDS Multi-AZ, ElastiCache Multi-AZ, EFS, S3
  - Site to Site VPN as a recovery from Direct Connect
- Replication
  - RDS replication (Cross Region), AWS Aurora + Global Databases
  - Database replication from on-premises to RDS
  - Storage Gateway
- Automation
  - CloudFormation / Elastic Beanstalk to re-create a whole new environment
  - Recover / Reboot EC2 instances with CloudWatch if alarm fail
  - AWS Lambda functions for customized automations
- Chaos
  - Netflix has a “simian-army” randomly terminating EC2

## DMS - Database Migration Service

- Quickly and securely migrate databases to AWS, resilient, self healing
- The source database remains available during the migration
- Supports:
  - Homogeneous migrations: ex Oracle to Oracle
  - Heterogeneous migrations: ex Microsoft SQL Server to Aurora
- Continuous Data Replication using CDC
- You must create an EC2 instance to perform the replication tasks

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c86b95f2-69c0-4e05-993f-9d4b17e4c40a/Untitled.png)

### DMS Sources and Targets

**SOURCES**:

- On-Premises and EC2 instances databases: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP, DB2
- Azure: Azure SQL Database
- Amazon RDS, all including Aurora
- Amazon S3
- DocumentDB

**TARGETS**:

- On-Premises and EC2 instances databases: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, SAP
- Amazon RDS
- Redshift, DynamoDB, S3
- OpenSearch Service
- Kinesis Data Streams
- Apache Kafka
- DocumentDB & Amazon Neptune
- Redis & Babelfish

### AWS Schema Conversion Tool (SCT)

- Convert your Database’s schema from one engine to another
- Example OLTP: (SQL Server or Oracle) to MySQL, PostgreSQL, Aurora
- Example OLAP: (Teradata or Oracle) to Amazon Redshift
- Prefer compute-intensive instances to optimize data conversions

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/172c61e8-c048-45e3-876d-9b2793c1d173/Untitled.png)

- **You do not need to use SCT if you are migrating the same DB engine**
  - Ex: On-Premise PostgreSQL ⇒ RDS PostgreSQL
  - The DB engine is still PostgreSQL (RDS is the platform)

### DMS - Continuous Replication

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5c86416f-199e-43fb-b513-4f88d5116876/Untitled.png)

### AWS DMS - Multi-AZ Deployment

- When Multi-AZ Enabled, DMS provisions and maintains a synchronously stand replica in a different AZ
- Advantages:
  - Provides Data Redundancy
  - Eliminates I/O freezes
  - Minimizes latency spikes

## RDS & Aurora MySQL Migrations

- RDS MySQL to Aurora MySQL
    - Option 1: DB Snapshots from RDS MySQL restored  as MySQL Aurora DB
    - Option 2: Create an Aurora Read Replica from your RDS MySQL, and when the replication lag is 0, promote it as its own DB cluster (can take time and cost $)
- External MySQL to Aurora MySQL
    - Option 1:
        - Use Percona XtraBackup to create a file backup in Amazon S#
        - Create an Aurora MySQL DB from Amazon S3
    - Option 2:
        - Create an Aurora MySQL DB
        - Use the mysqldump utility to migrate MySQL into Aurora (slower than S3 methd)
- Use DMS if both databsae are up and running