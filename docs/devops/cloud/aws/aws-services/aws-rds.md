# AWS Services - RDS (Relational Database Service)

Amazon RDS (Relational Database Service) is a managed database service that makes it easy to set up, operate, and scale
relational databases in the cloud. It automates time-consuming tasks such as provisioning, backups, patching,
monitoring, and scaling.

---

## Supported Database Engines

RDS supports multiple relational database engines:

- **Amazon Aurora** (MySQL- and PostgreSQL-compatible, AWS-optimized, high performance).
- **MySQL**.
- **PostgreSQL**.
- **MariaDB**.
- **Oracle Database**.
- **Microsoft SQL Server**.

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
