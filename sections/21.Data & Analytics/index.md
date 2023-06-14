## Amazon Athena

- Serverless query service to analyze data stored in Amazon S3
- Uses standard SQL language to query the files (built on Presto)
- Supports CSV, JSON, ORC, Avro, and Parquet
- Pricing: $5.00 per TB of data scanned
- Commonly used with Amazon Quicksight for reporting/dashboards
- Use cases: Business intelligence / analytics / reporting, analyze & query VPC Flow Logs, ELB Logs, CloudTrail trails, etc…
- Exam Tip: analyze data in S3 using serverless SQL, use Athena

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bad79c6b-44c9-49b2-ac37-aa82c0ba8557/Untitled.png)

### Performance Improvement

- Use **columnar data** for cost-savings (less scan)
  - Apache Parquet or ORC is recommend
  - Huge performance improvement
  - Use Glue to convert your data to Parquet or ORC
- **Compress data** for smaller retrievals (bzip2, gzip, lz4, snappy, zlip…)
- **Partition** datasets in S3 for easy querying on virtual columns
  - S3://yourBucket/pathToTable
    - /<PARTITION_COLUMN_NAME> = <VALUE>
  - Example: s3://athena-examples/flight/parquet/year=1991/month=1/day=1
- **Use larger files** (>128 MB) to minimize overhead

### Federated Query

- Allows you to run SQL queries across data stored in relational, non-relational, object, and custom data sources (AWS or on-premises)
- Uses Data Source Connectors that run on AWS Lambda to run Federated Queries (eg: CloudWatch Logs, DynamoDB, RDS…)
- Store the results back in Amazon S3

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f88b1d9-5b10-40ca-b92d-81ea0c629844/Untitled.png)

## RedShift

- Redshift based on PostgreSQL, but it’s not used for OLTP (Online Transactions Processing)
- **It’s OLAP** = online analytical processing (analytics and data warehousing)
- 10X better performance than other data warehouses, scale to PBs of data
- Columnar storage of data (instead of row based) & parallel query engine
- Pay as you go based on the instances provisioned
- Has a SQL interface for performing the queries
- BI tools such as Amazon Quicksight or Tableau integrate with it
- vs Athena: faster queries / joins / aggregations thanks to indexes

### RedShift Cluster

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca732597-7802-416b-bce5-ff23ff5d4ac4/Untitled.png)

- Leader node: for query planning, results aggregation
- Compute node: for performing the queries, send results to leader
- You provision the node size in advance
- You can used Reserved Instances for cost savings

### Snapshots & DR

- **Redshift has “Multi-AZ” mode for some clusters**
- Snapshots are point-in-time backups of a cluster, stored internally in S3
- Snapshots are incremental (only what has changed is saved)
- You can restore a snapshot into a new cluster
- Automated: every 8 hours, every 5GB, or on a schedule. Set retention between 1 to 35 days
- Manual: snapshot is retained until you delete it
- **You can configure Amazon Redshift to automatically copy snapshots (automated or manual) of a cluster to another AWS Region**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4852d898-f74d-40f6-b476-71e26ca47090/Untitled.png)

### Loading data into Redshift

- **Large inserts are MUCH better**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/043d9b97-b7ce-4ad3-9c9d-763330aed34c/Untitled.png)

### Redshift Spectrum

- Query data that is already in S3 without loading it
- Must have a Redshift cluster available to start the query
- The query is then submitted to thousands of Redshift Spectrum nodes

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8ae21f2c-99d0-4c98-aab8-c35498fbcaf3/Untitled.png)

## Amazon OpenSearch Service

- **_Amazon OpenSearch is successor to Amazon ElasticSearch_**
- In DynamoDB, queries only exist by primary key or indexes…
- **With OpenSearch, you can search any field, even partially matches**
- It’s common to use OpenSearch as a complement to another database
- Two modes: managed cluster or serverless cluster
- Does not natively support SQL (can be enabled via a plugin)
- Ingestion from Kinesis Data Firehose, AWS IoT, and CloudWatch Logs
- Security through Cognito & IAM, KMS encryption, TLS
- Comes with OpenSearch Dashboards (visualization)

### OpenSearch patterns DynamoDB

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f5c6c71-ca0f-4382-8b77-c23ad757336a/Untitled.png)

### OpenSearch patterns CloudWatch Logs

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/619831b2-630d-4011-87c1-1284a615755b/Untitled.png)

### OpenSearch patterns Kinesis Data Streams & Kinesis Data Firehose

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/78eec671-4ac7-4c66-97a5-9573282dabbe/Untitled.png)

## Amazon EMR

- EMR stands for “Elastic MapReduce”
- EMR helps creating **Hadoop Clusters (Big Data)** to analyze and process vast amount of data
- The clusters can be made of hundreds of EC2 instaces
- EMR comes bundled with Apache Spark, HBase, Presto, Flink…
- EMR takes care of all the provisioning and configuration
- Auto-scaling and integrated with Spot instances
- **Use cases: data processing, machine learning, web indexing, big data…**

### Node types & purchasing

- **Mater Node**: Managed the cluster, coordinate, manage health - long running
- **Core Node**: Run tasks and store data - long running
- **Task Node (optional)**: Just to run tasks - usually Spot
- **Purchasing options:**
  - On-demand: reliable, predicable, won’t be terminated
  - Reserved (min 1 year): cost savings (EMR will automatically use if available)
  - Spot Instances: cheaper, can be terminated, less reliable
- Can have long-running cluster, or transient (temporary) cluster

## Amazon QuickSight

- **Serverless machine learing-powered business intelligence service to create interactive dashboards**
- Fast, automatically scalable, embeddable, with per-session pricing
- Use cases:
  - Business analytics
  - Building visualizations
  - Perform ad-hoc analysis
  - Get business insights using data
- Integrated with RDS, Aurora, Athena, Redshift, S3…
- **In-memory computation using SPICE** engine if data is imported into QuickSight
- Enterprise edition: Possibility to setup **Column-Level security (CLS)**

### QuickSight Integrations

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/66357067-54a1-4695-bfb7-045cf5bb27e8/Untitled.png)

### Dashboard & Analysis

- Define Users (standard versions) and Groups (enterprise version)
  - These users & groups only exist within QuickSight, not IAM!!
- A _dashboard_…
  - is a read-only snapshot of an analysis that you can share
  - preserves the configuration of the analysis (filtering, parameters, controls, sort)
- **You can share the analysis or the dashboard with Users or Groups**
- To share a dashboard, you must first publish it
- Users who see the dashboard can also see the underlying data

## AWS Glue

- Managed **extract, transform, and load (ETL)** service
- Useful to prepare and transform data for analytics
- Fully serverless service

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/31d63caa-5e49-41ee-9202-15a6b03e3a61/Untitled.png)

### Convert data into Parquet format

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/489eedcd-fca5-4951-85f1-42050d181bd6/Untitled.png)

### Glue Data Catalog: catalog of datasets

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eb5fdd6c-eef2-4f3f-8d06-2bcf2b788aa5/Untitled.png)

### Glue - things to know at a high-level

- **Glue Job Bookmarks**: prevent re-processing old data
- **Glue Elastic Views**:
  - Combine and replicate data across multiple data stores using SQL
  - No custom code, Glue monitors for changes in the source data, serverless
  - Leverages a “virtual table” (materialized view)
- **Glue DataBrew**: clean and normalize data using pre-built transformation
- **Glue Studio**: new GUI to create, run and monitor ETL jobs in Glue
- **Glue Streaming ETL** (built on Apache Spark Structured Streaming): compatible with Kinesis Data Streaming, Kafka, MSK (managed Kafka).

## AWS Lake Formation

- **Data lake = central place to have all your data for analytics purposes**
- Fully managed service that makes it easy to setup a **data lake** in days
- Discover, cleanse, trasnform, and ingest data into your Data Lake
- It automates many complex manual steps (collecting, cleansing, moving, cataloging data,…) and de-duplicate (using ML Transforms)
- Combine structured and unstructured data in the data lake
- **Out-of-the-box source blueprints: S3, RDS, Relational & NoSQL DB…**
- **Fine-grained Access Control for your applications (row and column-level)**
- Buit on top of AWS Glue

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4d20c634-16cc-4583-b46d-d580b1509d9a/Untitled.png)

## Kinesis Data Analytics for SQL applications

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/622556fb-8a14-492d-a671-246dc7410016/Untitled.png)

### Kinesis Data Analytics (SQL application)

- Real-time analytics on **Kinesis Data Streams & Firehose** using SQL
- Add reference data from Amazon S3 to enrich streaming data
- Fully managed, no servers to provision
- Automatic scaling
- Pay for actual consumption rate
- Output:
  - Kinesis Data Streams: create streams out of the real-time analytics queries
  - Kinesis Data Firehose: send analytics query results to destinations
- Use cases:
  - Time-series analytics
  - Real-time dashboards
  - Real-time metrics

### Kinesis Data Analytics for Apache Flink

- Use Flink (Java, Scala or SQL) to process and analyze streaming data

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d595fb65-19cd-4659-b4f9-f719131e961b/Untitled.png)

- Run any Apache Flink application on a managed cluster on AWS
  - Provisioning compute resources, parallel computation, automatic scaling
  - application backups (implemented as checkpoints and snapshots)
  - Use any Apache Flink programming features
  - Flink does not read from Firehose (use Kinesis Analytics for SQL instead)

### AWS Lake Formation Centralized Permissions Example

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/845a54cd-0e9c-41d8-bc91-0c303d007f2a/Untitled.png)

## Amazon Managed Streaming for Apache Kafka (Amazon MSK)

- Alternative to Amazon Kinesis
- Fully managed Apache Kafka on AWS
  - Allow you to create, update, delete clusters
  - MSK creates & manageds Kafka brokers nodes & Zookeeper nodes for you
  - Deploy the MSK cluster in your VPC, multi-AZ (up to 3 for HA)
  - Automatic recovery from common Apache Kafka failures
  - Data is stored on EBS volumes **for as long as you want**
- **MSK Serverless**
  - Run Apache Kafka on MSK without managing the capacity
  - MSK automatically provisions resources and scales compute & storage

### Apache Kafka at a high level

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3e76b643-fc84-476e-96d8-2282a4e4d83b/Untitled.png)

### Kinesis Data Streams vs Amazon MSK

**Kinesis Data Streams**

- 1MB message size limit
- Data streams with Shards
- Shard Splitting & Merging
- TLS In-flight encryption
- KMS at-rest encryption

**Amazon MSK**

- 1MB default, configure for higher (ex: 10MB)
- Kafka Topics with Partitions
- Can only add partitions to a topic
- PLAINTEXT or TLS In-flight Encryption
- KMS at-rest encryption

### Amazon MSK Consumers

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3951dd95-c0b0-43fa-b1c5-1e80281d614b/Untitled.png)

## Big Data Ingestion Pipeline

- We want the ingestion pipeline to be fully serverless
- We want to collect data in real time
- We want to transform the data
- We want to query the transformed data using SQL
- The reports created using the queries should be in S3
- We want to load that data into a warehouse and create dashboards

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4891e576-9e6c-4a68-ab7e-adee247074f3/Untitled.png)

### Discussion

- IoT Core allows you to harvest data from IoT devices
- Kinesis is great for real-time data collection
- Firehose helps with data delivery to S3 in near real-time (1 minute)
- Lambda can help Firehose with data transformations
- Amazon S3 can trigger notifications to SQS
- Lambda can subscribe to SQS (we could have connecter S3 to Lambda(
- Athena is a serverless SQL service and results are stored in S3
- The reporting bucket contains analyzed data and can be used by reporting tool such as AWS QuickSight, Redshift, etc…