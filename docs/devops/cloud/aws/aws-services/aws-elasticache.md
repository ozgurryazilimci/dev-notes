# AWS Services - ElastiCache

## Overview

**Amazon ElastiCache** is a fully managed, in-memory caching service that supports **Redis** and **Memcached**.  
It improves application performance by retrieving data from fast, managed in-memory caches, instead of relying solely on
slower disk-based databases.

Use cases:

- Caching frequently accessed data (e.g., session data, user profiles).
- Real-time analytics.
- Pub/Sub messaging (Redis).
- Leaderboards, gaming applications.
- Distributed caching layer for web apps and microservices.

---

## Engine Options

### Redis

- In-memory key-value store with persistence support.
- Advanced features: replication, clustering, pub/sub, transactions.
- Supports data structures like strings, lists, sets, hashes, and sorted sets.
- Best for: caching, real-time analytics, leaderboards, queues.

### Memcached

- Simple, high-performance in-memory object cache.
- No persistence or advanced data types.
- Easier to scale horizontally.
- Best for: object caching, database query caching, session stores.

---

## Key Concepts

- **Cluster**: A collection of cache nodes.
- **Node**: A single cache instance.
- **Replication Group (Redis only)**: Primary/replica setup for high availability.
- **Shards (Redis only)**: Partitioning data across multiple nodes.
- **Parameter Groups**: Configure engine-specific settings.
- **Security Groups / VPC**: Control access to the cache cluster.

---

## Setting up ElastiCache from AWS Console

1. Go to **ElastiCache** in the AWS Management Console.
2. Select engine: **Redis** or **Memcached**.
3. Configure:
    - Cluster name.
    - Engine version.
    - Node type (memory/CPU).
    - Number of nodes (and shards for Redis).
    - Multi-AZ replication for high availability.
    - Subnet group and security groups (VPC required).
4. Enable **automatic backups** (Redis only).
5. Launch cluster.

---

## Connecting to ElastiCache

### From an EC2 instance in the same VPC:

1. Ensure the EC2 security group allows outbound access and ElastiCache security group allows inbound on port:
    - Redis: **6379**
    - Memcached: **11211**
2. Use native client libraries:
    - `redis-cli` for Redis
    - `memcached-tool` or any Memcached client library

---

## Example: Using Redis with Telnet

You can test connectivity and basic commands using `telnet`.

    telnet <elasticache-endpoint> 6379

Commands:

    SET mykey "HelloWorld"
    +OK

    GET mykey
    "HelloWorld"

If connection works, this confirms the cache cluster is up and responding.

---

## Example: Using Memcached with Telnet

    telnet <elasticache-endpoint> 11211

Commands:

    set mykey 0 900 5
    hello
    STORED

    get mykey
    VALUE mykey 0 5
    hello
    END

---

## Monitoring & Management

- **Amazon CloudWatch Metrics**:
    - CPUUtilization
    - FreeableMemory
    - Evictions
    - CurrConnections
- **Events**: Notifications about cluster/node changes.
- **Backups** (Redis only): Snapshots to S3.
- **Parameter Groups**: Tune engine configuration.
- **Maintenance Windows**: Apply patches automatically.

---

## High Availability & Scaling

- **Replication Groups (Redis)**: Primary + multiple replicas across AZs.
- **Auto-failover**: If the primary fails, a replica is promoted.
- **Cluster Mode Enabled (Redis)**: Sharded architecture for horizontal scaling.
- **Scaling**:
    - Vertically: Change node type to larger instance.
    - Horizontally: Add shards or replicas.

---

## Best Practices

- Place ElastiCache in the **same VPC and subnet** as your application for low latency.
- Use **Redis AUTH** or **VPC security groups** for access control.
- Enable **Multi-AZ with automatic failover** for production workloads.
- Use **parameter groups** to fine-tune performance.
- For session storage, set appropriate **TTL (time-to-live)** values.
- Regularly monitor **evictions** and **memory usage**.

---

## Useful CLI Commands

Check Redis connection:

    redis-cli -h <endpoint> -p 6379 ping

Check Memcached stats:

    echo stats | nc <endpoint> 11211

---

## Summary

- **ElastiCache** boosts performance by caching frequently accessed data.
- Choose **Redis** for advanced features and persistence, or **Memcached** for simple caching.
- Use **CloudWatch** for monitoring, **snapshots** for Redis backups, and **multi-AZ** for high availability.
- Test connectivity via `telnet`, `redis-cli`, or `nc`.  
