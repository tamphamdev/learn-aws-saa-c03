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