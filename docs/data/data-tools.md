# Data Tools

This document provides an overview of the main data platforms we use for analytics and data engineering, focusing on
**BigQuery**, **Snowflake**, and other commonly used tools.

---

## 1. Google BigQuery

### Overview

BigQuery is a **fully managed, serverless data warehouse** by Google Cloud. It enables fast SQL queries using the
processing power of Google’s infrastructure.

### Key Features

- **Serverless**: No infrastructure management.
- **Scalability**: Handles petabytes of data seamlessly.
- **Performance**: Distributed query engine for fast analytics.
- **Integration**: Works well with Google Cloud ecosystem (Dataflow, Pub/Sub, Looker, etc.).
- **Pricing**: Pay per query (on-demand) or flat-rate.

### Typical Use Cases

- Large-scale data analytics
- Real-time streaming data analysis
- Machine learning integration via BigQuery ML
- Building dashboards with Looker Studio / BI tools

### Example SQL

```sql
SELECT user_id,
    COUNT(*) AS total_actions
FROM `project.dataset.events`
WHERE event_date >= '2025-01-01'
GROUP BY user_id
ORDER BY total_actions DESC;
```

---

## 2. Snowflake

### Overview

Snowflake is a **cloud-native data warehouse** that separates storage and compute for flexibility and cost efficiency.
It runs on AWS, Azure, and Google Cloud.

### Key Features

- **Separation of Compute & Storage**: Scale independently.
- **Multi-Cloud**: Deploy across different cloud providers.
- **Data Sharing**: Share data securely across organizations.
- **Semi-Structured Data**: Supports JSON, Avro, Parquet.
- **Security**: Strong governance, role-based access control.

### Typical Use Cases

- Enterprise data warehousing
- Data lake + data warehouse hybrid (lakehouse)
- Secure data collaboration
- ELT/ETL workloads with dbt or custom pipelines

### Example SQL

```sql
SELECT customer_id,
    SUM(order_amount) AS total_spent
FROM orders
WHERE order_date >= '2025-01-01'
GROUP BY customer_id
ORDER BY total_spent DESC;
```

---

## 3. Other Common Tools

### dbt (Data Build Tool)

- Transformation layer for analytics engineering.
- Manages SQL models, testing, and documentation.
- Works with both BigQuery and Snowflake.

### Airflow

- Workflow orchestration for ETL/ELT pipelines.
- Schedule and monitor jobs.
- Integrates with GCP, AWS, Snowflake, and more.

### Looker / Looker Studio

- Business intelligence and data visualization tools.
- Connect directly to BigQuery and Snowflake.
- Used for dashboards, reporting, and self-service analytics.

### Fivetran / Stitch

- Managed data integration tools (ELT).
- Automates ingestion from SaaS apps, databases, and APIs.

---

## 4. Best Practices

- **Cost Management**
    - BigQuery: Use partitioned and clustered tables to optimize queries.
    - Snowflake: Scale warehouses appropriately, suspend unused ones.

- **Data Governance**
    - Define access roles and permissions carefully.
    - Keep sensitive data encrypted and masked where required.

- **Performance Optimization**
    - Avoid SELECT * in queries.
    - Use caching and materialized views where possible.

- **Documentation & Testing**
    - Maintain clear documentation of datasets and models.
    - Use dbt tests to validate data quality.

---

## Summary

- **BigQuery**: Best for serverless analytics and GCP-native projects.
- **Snowflake**: Flexible, enterprise-grade, multi-cloud data warehouse.
- **Supporting Tools**: dbt, Airflow, Looker, and Fivetran complete the data stack.

By combining these tools, we achieve a modern, scalable, and maintainable data ecosystem.
