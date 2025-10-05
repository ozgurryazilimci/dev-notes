# AWS Kinesis

Amazon Kinesis is a platform for real-time data streaming on AWS. It allows developers to collect, process, and analyze
streaming data such as logs, metrics, IoT events, clickstreams, and financial transactions.

This guide provides an overview of **Kinesis Streams**, **Kinesis Data Firehose**, and **Kinesis Data Analytics**, with
details on how to use them both via the **AWS Console (UI)** and programmatically as a developer.

---

## Core Components of Kinesis

### Kinesis Data Streams

- Enables ingestion of large volumes of real-time data.
- Data is captured as **shards**, which are units of parallelism.
- Producers push data into a stream, and consumers (applications) process the data.

### Kinesis Data Firehose

- Fully managed delivery service.
- Ingests real-time data and automatically delivers it to destinations such as:
    - Amazon S3
    - Amazon Redshift
    - Amazon OpenSearch Service
    - Custom HTTP endpoints

### Kinesis Data Analytics

- Enables SQL-based or Apache Flink-based stream processing.
- Allows developers to run real-time analytics without managing servers.

---

## Using AWS Console (UI)

### Create a Kinesis Data Stream

1. Go to the **AWS Management Console**.
2. Navigate to **Kinesis** → **Data Streams**.
3. Click **Create data stream**.
4. Enter:
    - **Stream name** (e.g., `my-stream`)
    - **Number of shards**
5. Click **Create data stream**.
6. Wait until the status shows **Active**.

### Create a Kinesis Firehose Delivery Stream

1. In the Kinesis console, go to **Data Firehose**.
2. Click **Create delivery stream**.
3. Choose **Source**:
    - Direct PUT
    - From a Kinesis Data Stream
4. Choose **Destination** (e.g., Amazon S3).
5. Configure buffering, compression, and encryption.
6. Click **Create delivery stream**.

### Create a Kinesis Analytics Application

1. Go to **Kinesis Data Analytics** in the console.
2. Click **Create application**.
3. Provide:
    - **Application name**
    - **Runtime environment** (SQL or Apache Flink)
4. Connect the application to a **streaming source**.
5. Write your **SQL queries** or **Flink job**.
6. Start the application to process streaming data in real time.

---

## Developer (SDK/CLI) Usage

### Writing Data to a Stream (Python SDK - boto3)

```python
import boto3
import json

# Create client
kinesis = boto3.client('kinesis')

# Send data
response = kinesis.put_record(
    StreamName='my-stream',
    Data=json.dumps({"event": "user_signup", "user_id": 123}),
    PartitionKey="user_123"
)
print(response)
```

### Consuming Data from a Stream (Python SDK)

```python
import boto3

kinesis = boto3.client('kinesis')

# Get shard iterator
shard_iterator = kinesis.get_shard_iterator(
    StreamName='my-stream',
    ShardId='shardId-000000000000',
    ShardIteratorType='TRIM_HORIZON'
)['ShardIterator']

# Read records
records = kinesis.get_records(ShardIterator=shard_iterator, Limit=10)
for record in records['Records']:
    print(record['Data'])
```

### Sending Data to Firehose

```python
firehose = boto3.client('firehose')

response = firehose.put_record(
    DeliveryStreamName='my-firehose',
    Record={'Data': b'{"message":"Hello Kinesis"}\n'}
)
print(response)
```

### AWS CLI Examples

```bash
# Create a new stream
aws kinesis create-stream --stream-name my-stream --shard-count 1

# Put a record
aws kinesis put-record     --stream-name my-stream     --partition-key user1     --data "eyJldmVudCI6ICJsb2dpbiJ9"  # Base64 encoded

# Get records
aws kinesis get-shard-iterator     --stream-name my-stream     --shard-id shardId-000000000000     --shard-iterator-type TRIM_HORIZON

aws kinesis get-records --shard-iterator <your-iterator>
```

---

## Best Practices

- **Partitioning**: Choose partition keys carefully to avoid hot shards.
- **Monitoring**: Use **CloudWatch metrics** to monitor throughput and shard usage.
- **Scaling**: Adjust shard count dynamically to handle higher traffic.
- **Security**:
    - Use **IAM roles and policies** for fine-grained access.
    - Enable **server-side encryption** for data in transit and at rest.
- **Error Handling**: Implement retry logic and dead-letter queues for failed events.

---

## Common Use Cases

- **Real-time log and metrics collection**
- **Clickstream analysis for web applications**
- **IoT device event ingestion**
- **Streaming ETL pipelines**
- **Fraud detection and anomaly detection**

---

## Summary

AWS Kinesis provides a powerful real-time data ingestion and analytics platform. Developers can use:

- **AWS Console (UI)** for quick setup and monitoring.
- **AWS SDKs & CLI** for integration into applications.

With Kinesis, you can reliably build **scalable, real-time applications** for streaming data analytics and processing.
