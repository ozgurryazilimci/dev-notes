# AWS Services - Redshift

Amazon Redshift is a fully managed, petabyte-scale data warehouse service designed for analytics and business
intelligence. It allows developers and data engineers to store and query large datasets quickly using standard SQL.

## OLTP vs OLAP

When working with databases and data warehouses, it’s important to understand the difference between **OLTP (Online
Transaction Processing)** and **OLAP (Online Analytical Processing)**:

### OLTP

- Designed for **transactional applications** (e.g., e-commerce, banking).
- Handles **many small, quick operations**: INSERT, UPDATE, DELETE.
- Focuses on **data integrity and fast transaction processing**.
- Example: MySQL, PostgreSQL, SQL Server.

### OLAP

- Designed for **analytical queries** and **business intelligence**.
- Handles **large-scale read-intensive operations**: SELECT with aggregations, joins, and reporting.
- Optimized for **query performance and complex analytics** rather than transaction speed.
- Example: Amazon Redshift, Snowflake, BigQuery.

Redshift is an **OLAP-focused service**, optimized for analytics and large-scale reporting rather than transactional
workloads.

## Redshift Architecture

- **Cluster:** The main unit of Redshift, containing one or more nodes.
    - **Leader Node:** Coordinates query processing and manages communication with client applications.
    - **Compute Nodes:** Store data and execute queries in parallel.
- **Node Types:** Dense Compute (DC) for performance, Dense Storage (DS) for large datasets.
- **Columnar Storage:** Optimized for analytical queries.
- **Massively Parallel Processing (MPP):** Distributes query execution across all nodes.

## SQL Support

- Redshift supports **standard SQL** for querying.
- Compatible with **PostgreSQL clients**, libraries, and tools.

## Data Loading and Export

- Load data from:
    - Amazon S3
    - DynamoDB
    - EMR or via JDBC/ODBC
- Use **COPY command** for fast data ingestion.
- Export query results to S3 or other AWS services using **UNLOAD command**.

## Security

- **Encryption:** At rest and in transit (KMS or HSM).
- **Network Isolation:** VPC, security groups, and cluster subnet groups.
- **IAM Integration:** Manage permissions and access control.

## Redshift Console Options

### Cluster Management

- **Create Cluster:** Choose node type, number of nodes, database name, username/password.
- **Modify Cluster:** Change node type, resize cluster, or adjust security settings.
- **Delete Cluster:** Optionally retain automated snapshots before deletion.

### Database & Schema Management

- Create databases, schemas, and users.
- Manage roles and permissions.

### Snapshots & Backup

- **Automated Snapshots:** Enable automatic backups with configurable retention.
- **Manual Snapshots:** Persistent backups you can restore anytime.
- **Restore:** Create a new cluster from a snapshot.

### Monitoring & Performance

- **CloudWatch Metrics:** CPU, disk, query performance, WLM queues.
- **Performance Insights:** Query execution details, bottlenecks, and recommendations.
- **Enhanced VPC Routing:** Control data traffic through VPC for security and compliance.

### Workload Management (WLM)

- Configure query queues, memory allocation, and concurrency.
- Define **short query acceleration** to prioritize small queries.

### Elastic Resize & Concurrency Scaling

- **Elastic Resize:** Quickly add or remove nodes without downtime.
- **Concurrency Scaling:** Automatically add transient clusters to handle peak loads.

## Redshift Spectrum

- Query data directly from S3 without loading into Redshift.
- Supports multiple formats (Parquet, ORC, CSV, JSON).

## Example Commands

- Connect to cluster using psql:
```bash
psql -h <cluster-endpoint> -U <username> -d <database-name> -p 5439
```
- Load data from S3:
```bash
COPY my_table
FROM 's3://my-bucket/data/'
IAM_ROLE '<role-arn>'
FORMAT AS PARQUET;
```

- Export data to S3:
```bash
UNLOAD ('SELECT * FROM my_table')
TO 's3://my-bucket/results/'
IAM_ROLE '<role-arn>'
PARALLEL OFF;
```

## Key Best Practices

- Sort and distribution keys optimize query performance.
- Use compression encodings to reduce storage and improve speed.
- Regularly monitor WLM queues to prevent query bottlenecks.
- Schedule snapshots and backups for data safety.
- Utilize Redshift Spectrum for large, infrequently queried datasets.

