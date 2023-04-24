# Overview

## Amazon RDS

### Amazon RDS Overview

- RDS stands for Relational Database Service
- It’s a managed DB service for DB use SQL as a query language
- It allows you to create databases in the cloud that are managed by AWS
    - Postgres
    - MySQL
    - MariaDB
    - Oracle
    - Microsoft SQL Server
    - Aurora(AWS Proprietary database)

### Advantage over using RDS versus deploying DB on EC2

- RDS is a managed service:
    - Automated provisioning, OS patching
    - Continuous backups and restore to specific timestamp (Point in Time Restore)
    - Monitoring dashboards
    - Read replicas for improved read performance
    - Multi AZ setup for DR (Disaster Recovery)
    - Maintenance windows for upgrades
    - Scaling capability (vertical and horizontal)
    - Storage backed by EBS (`gp2` or `io1`)
- BUT you can’t SSH into your instances

### RDS - Storage Auto Scaling

- Helps you increase storage on your RDS DB instance dynamically
- When RDS detects you are running out of free database storage, it scales automatically
- Avoid manually scaling your database storage
- You have to set **Maximum Storage Threshold** (maximum limit for DB storage)
- Automatically modify storage if:
    - Free storage is less than 10% of allocated storage
    - Low-storage lasts at least 5 minute
    - 6 hours have passed since last modification
- Useful for applications with **unpredictable workloads**
- Supports all RDS database engines (MariaDB, MySQLm PostgreSQL, SQL Server, Oracle)
### RDS Read Replicas for read scalability

- Up to 15 Read Replicas
- **Within AZ, Cross AZ, Cross Region**
- Replication is **ASYNC**, so reads are eventually consistent
- Replicas can be promoted to their own DB (có thể trở thành DB chính)
- Applications must update the connection string to leverage read replicas

### RDS Read Replicas - Use Cases

- You have a production database that is taking on normal load
- You want to run a reporting application to run some analytics
- You create a Read Replica to run the new workload there
- The production application is unaffected
- Read replicas are used for SELECT (=read) only kind of statements (not INSERT, UPDATE, DELETE)

### RDS Read Replicas - Network Cost

- In AWS there’s a network cost when data goes from one AZ to another
- **For RDS Read Replicas within the same region, you don’t pay that fee**

### RDS Multi AZ (Disaster Recovery)**

- SYNC replication
- One DNS name- automatic app failover to standby
- Increase availability
- Failover in case of loss of AZ, loss of network, instance or storage failure
- No manual intervention in apps
- Not used for scaling
- Note: T**he Read Replicas be setup as Multi AZ for Disaster Recovery (DR)**

### RDS - From Single-AZ to Multi-AZ

- Zero downtime operation (no need to stop the DB)
- Just click on “modify” for the database
- The following happens internally:
    - A snapshot is taken
    - A new DB is restored from the snapshot in a new AZ
    - Synchronization is established between the two databases
### RDS Custom

- **Managed Oracle and Microsoft SQL Server Database with OS and database customization**
- RDS: Automates setup, operation, and scaling of database in AWS
- Custom: access to the underlying database and OS so you can
    - Configure settings
    - Install patches
    - Enable native features
    - Access the underlying EC2 Instance using **SSH** or **SSM Session Manager**
- **De-activate Automation Mode** to perform your customization, better to take a DB snapshot before
- RDS vs RDS Custom
    - RDS: entire database and the OS to be managed by AWS
    - RDS custom: full admin access to the underlying OS and the database
## Amazon Aurora

### Overview

- Aurora is a proprietary technology from AWS (not open sourced)
- Postgres and MySQL are both supported as Aurora DB(that means your drivers will work as if Aurora was a Postgres or MySQL database)
- Aurora is “AWS cloud optimized” and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS
- Aurora storage automatically grows in increments of 10GB, up to 128TB
- Aurora can have 15 replicas while MySQL has 5, and the replication process is faster (sub 10ms replica lag)
- Failover in Aurora is instantaneous. It’s HA native (High Availability)
- Aurora costs more than RDS (20% more) - but is more efficient

### Aurora High Availability and Read Scaling

- 6 copies of your data across 3 AZ:
    - 4 copies out of 6 needed for writes
    - 3 copies out of 6 needed for reads
    - Self healing with peer-to-peer replication
    - Storage is striped across 100s of volumes

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1db4b909-503f-468e-978d-4c3323c0b5d6/Untitled.png)

- One Aurora Instance takes write (master)
- Automated failover for master in less than 30 seconds
- Master + up to 15 Aurora Read Replicas serve reads
- Support for Cross Region Replication

### Aurora DB Cluster

- Write endpoint (DNS name)
- Reader Endpoint ( How to know which replicas is reading)

### Feature of Aurora

- Automatic fail-over
- Backup and Recovery
- Isolation and security
- Industry compliance
- Push-button scaling
- Automated Patching with Zero Downtime
- Advanced Monitoring
- Routing Maintenance
- Backtrack: restore data at any point of time without using backups
### Aurora - Custom Endpoints

- Define a subset of Aurora Instances as a Custom Endpoint
- Example: run analytical queries on specific replicas
- The Reader Endpoint is generally not used after defining Custom Endpoint

### Aurora Serverless

- Automated database instantiation and auto scaling based on actual usage
- Good for infrequent intermittent or unpredictable workloads
- No capacity planning needed
- Pay per second, can be more cost-effective

### Aurora Multi-Master

- In case you want immediate failover for write-node (HA)
- Every node does R/W - vs promoting a RR (Replica Read) as a new master

### Global Aurora

- **Aurora Cross Region Read Replicas:**
    - Useful for disaster recovery
    - Simple to put in place
- **Aurora Global Database (recommended):**
    - 1 Primary Region (read / write)
    - Up to 5 secondary (read-only) regions, replication lag is less than 1 second
    - Up to 16 Read Replicas per secondary region
    - Helps for decreasing latency
    - Promoting another region (for disaster recovery) has an RTO (Recovery Time Objective) of < 1 minute
    - **Typical cross-region replication takes less than 1 second**
### Aurora Machine Learning

- Enables you to add ML-based predictions to your applications via SQL
- Simple, optimized, and secure integration between Aurora and AWS ML services
- Supported services
    - Amazon SageMaker (use with any ML model)
    - Amazon Comprehend (for sentiment analysis)
- You don’t need to  have ML experience
- Use cases: fraud detection, ads targeting, sentiment analysis, product recommendations
## Elastic Cache