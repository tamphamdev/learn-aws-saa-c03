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