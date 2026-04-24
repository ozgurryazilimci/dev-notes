# PostgreSQL Real-Time Data Streaming: Publications, Slots, and Sequin

This guide explains the architecture of real-time data synchronization in PostgreSQL, focusing on
**Logical Replication**, **Replication Slots**, and how modern tools like **Sequin** leverage
these features to build event-driven systems.

---

## 1. Logical Replication Fundamentals

PostgreSQL offers two types of replication: Physical and Logical. For data streaming and CDC (Change Data Capture), we
use **Logical Replication**.

### What is a Publication?

A **Publication** is a node on the Primary database that defines which tables and changes (INSERT, UPDATE, DELETE)
should be broadcasted.

- **Scope:** You can publish all tables or a specific subset.
- **Granularity:** You can choose to publish only specific operations.

```sql
-- Create a publication for specific tables
CREATE
PUBLICATION my_app_publication FOR TABLE users, orders;

-- Create a publication for all tables
CREATE
PUBLICATION all_tables_pub FOR ALL TABLES;
```

---

## 2. The Role of Replication Slots

While a Publication defines *what* is sent, a **Replication Slot** manages *how* it is delivered and ensures no data is
lost.

### Why do we need Slots?

In PostgreSQL, transaction logs (WAL - Write Ahead Logs) are recycled periodically. If a consumer (like Sequin) goes
offline, the logs it needs might be deleted.

**A Replication Slot ensures:**

1. **Persistence:** Tells the Primary DB: "Do not delete these logs until the consumer confirms receipt."
2. **Cursor Management:** Keeps track of the **LSN (Log Sequence Number)**, acting as a pointer to where the consumer
   last stopped.

> **Warning:** If a consumer stays offline for too long, the Replication Slot will cause WAL files to accumulate,
> potentially filling up the Primary server's disk.

---

## 3. Enter Sequin: The Modern Bridge

**Sequin** is an open-source Change Data Capture (CDC) tool that connects to your PostgreSQL database and streams
changes to external targets like Webhooks, SQS, Kafka or Redis.

### How Sequin Uses the Stack

1. **Subscription:** Sequin connects to a PostgreSQL **Publication**.
2. **State Tracking:** It creates its own **Logical Replication Slot** to ensure it never misses a single database
   event.
3. **Transformation:** It converts the internal PostgreSQL binary stream into easy-to-use **JSON** payloads.

**Architecture Flow:**
PostgreSQL (Table) -> Publication -> Replication Slot -> Sequin -> Your API/App

---

## 4. Configuration & Setup Checklist

To use this architecture, your `postgresql.conf` must be configured as follows:

```
# Required for Logical Replication and Sequin
wal_level = logical
max_replication_slots = 10
max_wal_senders = 10
```

### Useful Monitoring Queries

**Check active publications:**

```sql
SELECT *
FROM pg_publication;
```

**Monitor replication slots and lag:**

```sql
SELECT slot_name,
       active,
       restart_lsn,
       pg_size_pretty(pg_current_wal_lsn() - restart_lsn) AS replication_lag
FROM pg_replication_slots;
```

---

## Conclusion

By combining **Publications** for data selection, **Replication Slots** for reliability, and **Sequin** for delivery,
you can transform your static PostgreSQL database into a powerful, real-time event source.