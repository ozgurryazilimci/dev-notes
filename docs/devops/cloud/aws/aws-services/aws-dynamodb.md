# AWS Services - DynamoDB

Amazon DynamoDB is a **fully managed NoSQL database service** that provides fast and predictable performance with
seamless scalability. Unlike SQL databases, DynamoDB does not use tables with joins and complex schemas but instead
focuses on **key-value** and **document** data models.

---

## Key Features

- **NoSQL Database**: Key-value and document-based.
- **Fully Managed**: No server provisioning, patching, or scaling required.
- **Performance**: Single-digit millisecond latency.
- **Scalability**: Auto-scaling with on-demand capacity.
- **Availability**: Multi-AZ replication by default.
- **Security**: Encryption at rest and in transit, IAM-based access control.
- **Integration**: Works seamlessly with AWS Lambda, Streams, API Gateway, S3, etc.

---

## Core Concepts

### Tables

- A collection of data in DynamoDB.
- Each table must have a **primary key** defined.

### Items

- Equivalent to a row in SQL.
- Each item is a set of attributes (key-value pairs).

### Attributes

- Equivalent to columns in SQL.
- DynamoDB allows flexible attributes — items in the same table may have different attributes.

### Primary Key

Two options:

1. **Partition Key** (simple primary key)
    - A single attribute used to uniquely identify each item.
    - Example: `UserId`.
2. **Partition Key + Sort Key** (composite primary key)
    - Uniqueness is determined by the combination of partition and sort key.
    - Example: `UserId` (partition key) + `OrderDate` (sort key).

### Secondary Indexes

- Allow querying data with alternative keys.

1. **Global Secondary Index (GSI)**
    - Partition + sort key can be different from the base table.
2. **Local Secondary Index (LSI)**
    - Uses the same partition key as the base table but a different sort key.

### Provisioned vs On-Demand Capacity

- **Provisioned**: You define Read Capacity Units (RCUs) and Write Capacity Units (WCUs).
- **On-Demand**: Pay per request; DynamoDB auto-scales.

---

## Storage and Performance

- Data is stored across **multiple partitions** automatically.
- Performance is tied to **partition keys**:
    - **Hot partition problem** occurs if one key is accessed disproportionately.
    - Use good partition key design to distribute load.
- **DynamoDB Accelerator (DAX)**: In-memory caching layer for microsecond response times.

---

## Common Features

### Streams

- Capture item-level changes (insert, update, delete).
- Commonly used with AWS Lambda for event-driven architectures.

### Time to Live (TTL)

- Automatically deletes expired items based on a timestamp attribute.

### Encryption

- Default: AWS-managed KMS key.
- Option: Use customer-managed CMKs.

### Transactions

- ACID transactions across multiple items and tables.

### Backups

- **On-demand backups**: Full backups.
- **Point-in-time recovery (PITR)**: Continuous backup with restore to any second in the last 35 days.

---

## AWS Console Options for DynamoDB

1. **Tables**
    - Create and manage tables.
    - Define partition and sort keys.
    - Configure capacity (on-demand or provisioned).
    - Enable Streams, TTL, encryption.

2. **Items**
    - Insert, edit, delete items directly.
    - Run queries and scans.

3. **Indexes**
    - Create and monitor GSIs and LSIs.

4. **Backups and Restore**
    - Manage PITR and on-demand backups.
    - Restore a backup into a new table.

5. **Metrics & Monitoring**
    - CloudWatch metrics: throttled reads/writes, consumed capacity.
    - DynamoDB console dashboards.

6. **Global Tables**
    - Multi-region replication for disaster recovery and low-latency global access.

---

## Example CLI Commands

Create a new table:

```bash
aws dynamodb create-table
--table-name Users
--attribute-definitions AttributeName=UserId,AttributeType=S
--key-schema AttributeName=UserId,KeyType=HASH
--billing-mode PAY_PER_REQUEST
```

Insert an item:

```bash
aws dynamodb put-item
--table-name Users
--item '{"UserId": {"S": "123"}, "Name": {"S": "Alice"}}'
```

Query items:

```bash
aws dynamodb query
--table-name Users
--key-condition-expression "UserId = :id"
--expression-attribute-values '{":id":{"S":"123"}}'
```

---

## Best Practices

- Design tables based on **access patterns**, not just data storage.
- Use **composite keys** to allow flexible queries.
- Avoid hot partitions by distributing partition keys evenly.
- Use **DAX** for read-heavy workloads.
- Enable **PITR backups** for resilience.
- Use **Streams + Lambda** for serverless event processing.

---

## DynamoDB Use Cases

- Gaming leaderboards.
- Session management.
- IoT device data storage.
- Shopping carts.
- Event logging and analytics.
