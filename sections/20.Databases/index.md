## Choosing the Right Database

- We have a lot of managed databases on AWS to choose from
- Questions to choose the right database based on your architecture:
    - Read-heavy, write-heavy, or balanced workload? Throughput needs? Will it change, does it need to scale or fluctuate during the day?
    - How much data to store and for how long? Will it grow? Average object size? How are. they accessed?
    - Data durability? Source of truth for the data?
    - Latency requirements? Concurrent users?
    - Data model? How will you query the data? Joins? Structured? Semi-Structured?
    - Strong schema? More flexibility? Reporting? Search? RDBMS/ NoSQL?
    - License costs? Switch to Cloud Native DB such as Aurora?

## Database Types

- **RDBMS (=SQL /OLTP)**: RDS, Aurora - great joins
- **NoSQL database - no joins, no SQL**: DynamoDB(~JSON), ElasticCache(key/value pairs), Neptune (graphs), DocumentDB (for MongoDB), Keyspaces (for Apache Cassandra)
- **Object Store**: S3 (for big objects) / Glacier (for backups / archives)
- **Data Warehouse** (= SQL Analytics / BI): Redshift (OLAP), Athena, EMR
- **Search**: OpenSearch (JSON) - free text, unstructured searches
- **Graphs**: Amazon Neptune - displays relationships between data
- **Ledger**: Amazon Quantum Ledger Database
- **Time series**: Amazon Timestream
- Note: some database are being discussed in the Data & Analytics section

## Amazon RDS

- Managed PostgreSQL / MySQL / Oracle / SQL Server / MariaDB / Custom
- Provisioned RDS Instance Size and EBS Volume Type & Size
- Auto-scaling capability for Storage
- Support for Read Replicas and Multi AZ
- Security through IAM, Security Groups, KMS, SSL in transit
- Automated Backup with Point in time restore feature (up to 35 days)
- Manual DB Snapshot for longer-term recovery
- Managed and Scheduled maintenance (with downtime)
- Support IAM Authentication, integration with Secrets Manager
- RDS Custom for access to and customize the underlying instance (Oracle & SQL Server)
- **Use cases**: Store relational datasets (RDBMS / OLTP), perform SQL queries, transactions

## Amazon Aurora

- Compatible API for PostgreSQL / MySQL, separation of storage and compute
- Storage: data is stored in 6 replicas, across 3 AZ - highly available, self-healing, auto-scaling
- Compute: Cluster of DB Instance across multiple AZ, auto-scaling of Read Replicas
- Cluster: Custom endpoints for writer and reader DB instances
- Same security / monitoring / maintenance features as RDS
- Know the backup & restore options for Aurora
- **Aurora Serverless** - for unpredictable / intermittent workloads, no capacity plaining
- **Aurora Multi-Master** - for continuous writes failover (high write availability)
- **Aurora Global**: yup to 16 DB Read Instances in each region, < 1 second storage replication
- **Aurora Machine Learning**: perform ML using SageMaker & Comprehend on Aurora
- **Aurora Database Cloning**: new cluster from existing one, faster than restoring a snapshot
- **Use cases**: same as RDS, but with less maintenance / more flexibility / more performance / more features

## Amazon ElastiCache

- Managed Redis / Memcached (similar offering as RDS, but for caches)
- In-memory data store, sub-millisecond latency
- Select an ElastiCache instance type (eg: cache.m6g.large)
- Support for Clustering (Redis) and Multi AZ, Read Replicas (sharding)
- Security through IAM, Security Groups, KMS, Redis Auth
- Backup / Snapshot / Point in time restore feature
- Managed and Scheduled maintenance
- **Requires some application code changes to be leveraged**
- **Use case**: Key/value store/, Frequent reads, less writes, cache results for DB queries, store session data for websites, cannot use SQL

## Amazon DynamoDB

- AWS proprietary technology, managed serverless NoSQL database, millisecond latency
- Capacity modes: provisioned capacity with optional auto-scaling or on-demand capacity
- Can replace ElastiCache as a key/value store (storing session data for example, using TTL feature)
- Highly Available, Multi AZ by default, Read an Writes are decoupled, transaction capability
- DAX cluster for read cache, microsecond read latency
- Security, authentication and authorization is done through IAM
- Event Processing: DynamoDB Streams to integrate with AWS Lambda, or Kinesis Data Streams
- Global Table feature: active-active setup
- Automated backups up to 35 day with PITR (restore to new table), or on-demand backups
- **Export to S3 without using RCU within the PITR window, import from S3 without using WCU**
- **Great to rapidly evolve schemas**
- **Use case**: Severless applications development (small documents 100s KB), distributed serverless cache