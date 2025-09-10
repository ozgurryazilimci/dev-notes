# AWS Services - RDS (Relational Database Service)

Amazon RDS (Relational Database Service) is a managed database service that makes it easy to set up, operate, and scale
relational databases in the cloud. It automates time-consuming tasks such as provisioning, backups, patching,
monitoring, and scaling.

---

## SQL vs NoSQL Databases

- **SQL (Relational Databases)**
    - Structured, table-based, predefined schema.
    - Strong ACID compliance (Atomicity, Consistency, Isolation, Durability).
    - Suitable for transactional systems, financial apps, traditional enterprise workloads.
    - Examples in AWS: **Amazon RDS** (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server), **Amazon Aurora**.

- **NoSQL (Non-Relational Databases)**
    - Flexible schema, key-value, document, graph, or column-oriented.
    - Focus on scalability and high availability.
    - Eventual consistency models.
    - Example in AWS: **Amazon DynamoDB**.

---

## Supported Database Engines

RDS supports multiple relational database engines:

- **Amazon Aurora** (MySQL- and PostgreSQL-compatible, AWS-optimized, high performance)
- **MySQL**
- **PostgreSQL**
- **MariaDB**
- **Oracle Database**
- **Microsoft SQL Server**

Each engine has specific versions and licensing options available.

---

## Key Concepts

- **DB Instance**: A database environment in RDS, running one database engine.
- **Multi-AZ Deployment**: Provides high availability by synchronously replicating data to a standby instance in another
  Availability Zone. Automatic failover is supported.
- **Read Replicas**: Asynchronous replication for read scaling and offloading queries. Available for MySQL, MariaDB,
  PostgreSQL, and Aurora.
- **Storage Types**:
    - General Purpose SSD (gp2/gp3).
    - Provisioned IOPS SSD (io1/io2) for high-performance workloads.
    - Magnetic (legacy, not recommended).
- **DB Parameter Groups**: Manage engine configurations.
- **Option Groups**: Enable features like Oracle TDE or SQL Server transparent data encryption.

---

## Instance Classes

- RDS supports various instance families optimized for compute, memory, or burstable workloads.
- Examples:
    - **t3/t4g**: Burstable instances for dev/test.
    - **m5/m6g**: General-purpose workloads.
    - **r5/r6g**: Memory-optimized workloads.
- Always size instances according to workload needs and monitor with CloudWatch.

---

## Creating a New RDS Instance

When creating a new RDS database via the AWS Console:

1. **Choose Engine**: Select Aurora, MySQL, PostgreSQL, etc.
2. **Templates**: Development/Test, Production, or Free Tier.
3. **Settings**: DB identifier, master username, and password.
4. **Instance Configuration**: Select instance class (vCPU and memory).
5. **Storage**: Choose type (General Purpose SSD, Provisioned IOPS).
6. **Connectivity**:
    - VPC and subnet group
    - Public access or private only
    - Security groups for inbound rules
    - IAM role for S3 import/export

7. **Additional Configurations**:
    - Initial database name
    - DB parameter groups
    - Option groups
    - Encryption settings

8. **Monitoring**: Enable Enhanced Monitoring or Performance Insights.
9. **Backup**: Automated backup retention period, backup window.
10. **Maintenance**: Automatic minor version upgrades, maintenance window.

---

## RDS Dashboard

In the AWS Management Console, the **RDS Dashboard** provides access to:

- **Databases**: Active instances and clusters.
- **Performance Insights**: Visualize query performance.
- **Snapshots**: Automated and manual snapshots.
- **Automated Backups**: Recovery points available for PITR.
- **Reserved Instances**: Purchase long-term capacity at discounted rates.
- **Subnet Groups**: Control which subnets RDS can use.
- **Parameter Groups**: Define engine configuration settings.
- **Option Groups**: Enable advanced features like Oracle TDE or SQL Server Audit.
- **Events and Subscriptions**: Notifications about changes or issues.
- **Recommendations**: Suggestions for optimizing performance, scaling, or updates.

---

## RDS Actions in the Console

When you select a database in the AWS RDS console, the **Actions menu** provides several options:

- **Modify**: Change instance size, storage, networking, or engine version. Often results in creating a new instance
  behind the scenes.
- **Reboot**: Restart the database engine without changing configuration.
- **Promote Read Replica**: Convert a read replica into a standalone, writable DB.
- **Take Snapshot**: Create a manual snapshot at any point in time.
- **Restore to Point in Time**: Recover the database using automated backups and transaction logs.
- **Delete DB Instance**: Permanently remove the DB instance. Automated backups are deleted, but manual snapshots
  remain.

---

## Storage and Backups

- **Automated Backups**:
    - Daily snapshots + transaction logs.
    - Retention: 1 to 35 days.
    - Used for point-in-time recovery.
- **Manual Snapshots**:
    - User-initiated, stored until deleted.
    - Can be shared across accounts or copied across regions.
- **Storage Auto Scaling**: Automatically increase storage size when capacity is near full.

---

## Security

- **Encryption**:
    - At-rest encryption with KMS keys.
    - In-transit encryption with SSL/TLS.
- **Network Isolation**:
    - Place RDS inside a VPC.
    - Publicly accessible or private-only depending on requirements.
- **Authentication**:
    - Traditional username/password.
    - IAM Database Authentication (for MySQL and PostgreSQL).
- **Access Control**:
    - Managed via Security Groups and Network ACLs.

---

## Monitoring & Maintenance

- **Amazon CloudWatch**: CPU, memory, storage, IOPS, connections.
- **Enhanced Monitoring**: Real-time OS-level metrics.
- **Performance Insights**: Advanced query performance monitoring.
- **Maintenance Window**: Define when RDS applies patches and updates.

### Logs

Depending on the database engine, RDS supports:

- **Error Logs**: Engine-specific error messages.
- **General Logs**: Records of client connections and queries.
- **Slow Query Logs**: Queries that exceed a performance threshold.
- **Audit Logs**: Security-relevant events.

These logs can be viewed in the console or exported to **Amazon CloudWatch Logs** for analysis and retention.

---

## Scaling

- **Vertical Scaling**: Change instance type or storage size. Requires downtime in some cases.
- **Horizontal Scaling**:
    - Read replicas for scaling read-heavy workloads.
    - Aurora offers auto-scaling for replicas.
- **Storage Scaling**: Enabled with auto-scaling or manual adjustments.

---

## Networking

- **VPC Integration**: RDS instances must be launched in a VPC.
- **Subnet Groups**: Define which subnets (AZs) RDS can use.
- **Endpoints**:
    - Instance Endpoint: Connects to a specific DB instance.
    - Cluster Endpoint (Aurora): Connects to the primary writer.
    - Reader Endpoint (Aurora): Load balances across replicas.

---

## High Availability & Disaster Recovery

- **Multi-AZ**:
    - Automatic failover from primary to standby instance.
    - Synchronous replication for durability.
- **Cross-Region Replication**:
    - For disaster recovery and global applications.
    - Aurora Global Database offers low-latency global replication.

---

## Maintenance & Operations

- **Patching**: AWS automatically applies patches during the maintenance window.
- **Parameter Groups**: Fine-tune DB behavior.
- **Option Groups**: Add advanced features like Oracle advanced security.
- **Event Subscriptions**: Get notified via SNS for DB events (failover, backup, restore).

---

## Common Use Cases

- **Web Applications**: Back-end databases for websites or APIs.
- **Analytics**: Using replicas for BI and reporting.
- **E-commerce**: Reliable transaction processing with Multi-AZ.
- **Mobile Apps**: Scalable backend databases with global replication.

---

## Best Practices

- Use **Multi-AZ** for production workloads.
- Enable **automated backups** and regularly test restores.
- Use **encryption (KMS)** for compliance.
- Place RDS instances in **private subnets** unless public access is required.
- Monitor performance with **Performance Insights** and optimize queries.
- Use **read replicas** or Aurora for scaling reads.
- Regularly update and test **parameter groups** and security rules.
