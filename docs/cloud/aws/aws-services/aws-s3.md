# AWS Services - S3

Amazon Simple Storage Service (Amazon S3) is a scalable object storage service designed to store and retrieve any amount
of data from anywhere on the web. This document provides an overview of the **AWS Management Console** for S3, including
bucket configuration, properties, features, and usage scenarios.

---

## Overview

Amazon S3 stores data as objects inside **buckets**. Each object consists of:

- A **key** (unique identifier, similar to a filename/path)
- The object’s **data** (actual file content)
- **Metadata** (system-defined and user-defined key-value pairs)
- **Optional access permissions**

The S3 console provides a graphical way to manage buckets, objects, security, and advanced configurations.

---

## Buckets

### Creating Buckets

Steps to create a bucket in the AWS console:

1. Go to **Amazon S3** in the AWS Management Console.
2. Click **Create bucket**.
3. Provide:
    - **Bucket name**
    - **AWS Region**
    - **Bucket settings** (options like versioning, encryption, etc.)
4. Configure **Block Public Access**.
5. Create the bucket.

### Naming Rules

- Must be globally unique.
- Must be DNS-compliant (lowercase letters, numbers, and hyphens).
- Must not contain uppercase letters or underscores.
- Cannot be changed after creation.

### Bucket Properties

- **Versioning**: Keeps multiple versions of an object, useful for rollback or accidental deletion recovery.
- **Encryption**: Ensures that all data is encrypted at rest (SSE-S3, SSE-KMS, etc.).
- **Logging**: Records access requests made to the bucket for auditing/security.
- **Tags**: Key-value pairs used for cost allocation and organization.
- **Events**: Configure notifications (to Lambda, SNS, SQS) on object-level events.
- **Replication**: Automatically copy objects to another bucket (in the same or different region).

---

## Objects

### Uploading Files

Options in the console:

- Drag and drop files/folders.
- Define object-level permissions during upload.
- Select **Storage Class** (affects cost and retrieval performance).
- Choose **Server-Side Encryption**.

### Object Properties

- **Key (Name)**: Identifier of the object.
- **Size**: File size in bytes.
- **Last modified**: Timestamp of the latest update.
- **Storage class**: Defines cost, availability, and retrieval times.
- **ETag**: A hash of the object (useful for validation).

### Object Versioning

- Once enabled, every change creates a new version of the object.
- Deleted objects are marked with a **delete marker** instead of being removed permanently.
- Helps with recovery from accidental deletions/overwrites.

---

## Access Management

### Bucket Policies

- JSON-based rules attached directly to a bucket.
- Bucket policies define access permissions in JSON.
- Allow fine-grained access control, including cross-account permissions.

Example: Allow only a specific IAM user to read objects in the bucket:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowUserReadOnly",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::111122223333:user/DeveloperUser"
      },
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::example-bucket/*"
    }
  ]
}
```

Example: Allow public read-only access (⚠️ not recommended for sensitive data):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example-bucket/*"
    }
  ]
}
```

### IAM Policies

- Define permissions at the **user/role/group** level.
- Work across all AWS services (not just S3).
- Recommended for managing permissions at scale.

### Access Control Lists (ACLs)

- Legacy mechanism for granting access.
- Should be avoided when possible; use bucket policies or IAM instead.

### Block Public Access

- Global setting at both **account** and **bucket** level.
- Prevents accidental exposure of data to the internet.
- Recommended to keep enabled for all production buckets.

---

## Security

### Encryption

- **SSE-S3**: Server-side encryption using S3-managed keys (AES-256).
- **SSE-KMS**: Server-side encryption using AWS Key Management Service (supports key rotation, fine-grained access
  control).
- **SSE-C**: Customer-provided keys.
- **Client-Side Encryption**: Data is encrypted before upload.

### S3 Object Lock

- Prevents objects from being deleted or modified for a specified retention period.
- Useful for compliance (WORM: Write Once, Read Many).

### Logging and Monitoring

- **Server Access Logging**: Tracks requests made to your bucket, stored as logs in another bucket.
- **AWS CloudTrail Data Events**: Logs API calls for S3, useful for security audits.
- **Amazon CloudWatch Metrics**: Provides performance metrics (e.g., request counts, latency, errors).

---

## Storage Management

### Storage Classes

Each object can be assigned a storage class:

- **S3 Standard**: High durability, high availability, low latency. Default for frequently accessed data.
- **S3 Intelligent-Tiering**: Automatically moves objects between frequent and infrequent access tiers based on usage.
- **S3 Standard-IA (Infrequent Access)**: Lower cost for infrequently accessed data, but with retrieval fees.
- **S3 One Zone-IA**: Cheaper but stores data in a single AZ (less resilient).
- **S3 Glacier**: Low-cost archival storage; retrieval times range from minutes to hours.
- **S3 Glacier Deep Archive**: Lowest-cost archival; retrieval can take up to 12–48 hours.

### Lifecycle Rules

- Automate storage class transitions (e.g., move to Glacier after 90 days).
- Define object expiration (permanently delete objects after a set time).
- Help optimize storage costs.

Example: Move objects to **S3 Standard-IA** after 30 days, then to **Glacier Deep Archive** after 180 days, and finally
delete after 365 days:

```json
{
  "Rules": [
    {
      "ID": "TransitionAndExpire",
      "Status": "Enabled",
      "Filter": {
        "Prefix": ""
      },
      "Transitions": [
        {
          "Days": 30,
          "StorageClass": "STANDARD_IA"
        },
        {
          "Days": 180,
          "StorageClass": "DEEP_ARCHIVE"
        }
      ],
      "Expiration": {
        "Days": 365
      }
    }
  ]
}
```

Example: Expire all objects with prefix `logs/` after 90 days:

```json
{
  "Rules": [
    {
      "ID": "ExpireLogs",
      "Status": "Enabled",
      "Filter": {
        "Prefix": "logs/"
      },
      "Expiration": {
        "Days": 90
      }
    }
  ]
}
```

### Replication

- **Same-Region Replication (SRR)**: Copy objects to another bucket in the same region.
- **Cross-Region Replication (CRR)**: Copy objects across regions for disaster recovery.
- Requires enabling **versioning** on source and destination buckets.
- Can replicate metadata, ACLs, and tags.

Example: **Cross-Region Replication (CRR)** configuration:

```json
{
  "Role": "arn:aws:iam::111122223333:role/ReplicationRole",
  "Rules": [
    {
      "Status": "Enabled",
      "Priority": 1,
      "DeleteMarkerReplication": {
        "Status": "Disabled"
      },
      "Filter": {
        "Prefix": ""
      },
      "Destination": {
        "Bucket": "arn:aws:s3:::destination-bucket",
        "StorageClass": "STANDARD_IA"
      }
    }
  ]
}
```

Example: Replicate only objects with prefix `images/`:

```json
{
  "Role": "arn:aws:iam::111122223333:role/ReplicationRole",
  "Rules": [
    {
      "Status": "Enabled",
      "Priority": 1,
      "Filter": {
        "Prefix": "images/"
      },
      "Destination": {
        "Bucket": "arn:aws:s3:::image-backup-bucket"
      }
    }
  ]
}

```

---

## Performance and Data Transfer

### Multipart Uploads

- Splits large files (over 100 MB) into smaller parts.
- Parallel uploads improve speed and reliability.
- Required for objects larger than 5 GB.

### Transfer Acceleration

- Uses Amazon CloudFront’s edge locations to accelerate uploads/downloads.
- Best for cross-continent data transfer.

---

## Advanced Features

### Event Notifications

- Trigger actions when objects are created, deleted, or updated.
- Targets:
    - **Amazon SNS** (notifications)
    - **Amazon SQS** (messaging queues)
    - **AWS Lambda** (serverless processing)

Example: Send an event to **SNS** when a new object is created:

```json
{
  "LambdaFunctionConfigurations": [],
  "QueueConfigurations": [],
  "TopicConfigurations": [
    {
      "Id": "NewObjectCreated",
      "TopicArn": "arn:aws:sns:us-east-1:111122223333:MySNSTopic",
      "Events": [
        "s3:ObjectCreated:*"
      ]
    }
  ]
}
```

Example: Trigger a **Lambda function** when an object with prefix `uploads/` is uploaded:

```json
{
  "LambdaFunctionConfigurations": [
    {
      "Id": "ProcessUploads",
      "LambdaFunctionArn": "arn:aws:lambda:us-east-1:111122223333:function:ProcessUpload",
      "Events": [
        "s3:ObjectCreated:*"
      ],
      "Filter": {
        "Key": {
          "FilterRules": [
            {
              "Name": "prefix",
              "Value": "uploads/"
            }
          ]
        }
      }
    }
  ]
}

```

### Requester Pays

- Shifts the cost of **data transfer** and **requests** to the requester, not the bucket owner.
- Useful for public datasets.

### Inventory Reports

- Generate CSV/ORC/Parquet files listing objects and metadata.
- Useful for auditing, compliance, and large-scale reporting.

### Batch Operations

Amazon S3 **Batch Operations** allows you to perform large-scale operations on many objects at once. This is especially
useful for tasks like copying objects, replacing tags, invoking Lambda functions, or modifying object ACLs across
thousands or millions of objects.

**Key points:**

- Works on objects listed in a manifest (CSV or S3 Inventory report).
- Supports operations like `Copy`, `PutObjectAcl`, `InitiateRestore`, `LambdaInvoke`.
- Allows monitoring progress and retries for failed operations.
- Can be scheduled or run on-demand via the console.

**Example Use Case:**

- Add a new tag to all objects in a bucket.
- Trigger a Lambda function on all archived objects to generate thumbnails or process data.
- Restore multiple Glacier objects simultaneously.

---

## Best Practices

- Enforce **least-privilege access** with IAM.
- Keep **Block Public Access** enabled.
- Use **bucket versioning** with **MFA delete** for critical data.
- Encrypt all data (SSE-KMS or SSE-S3).
- Implement **lifecycle policies** to optimize costs.
- Monitor using **CloudTrail** and **CloudWatch**.

---

## Common Use Cases

- **Static website hosting** (without server-side logic).
- **Backup and archival** (long-term retention with Glacier).
- **Data lakes** for analytics (integration with Athena, Redshift, EMR).
- **Content distribution** (images, videos, software packages).
- **Machine learning pipelines** (store datasets and training models).

---

## References

- [AWS S3 Documentation](https://docs.aws.amazon.com/s3/)
- [AWS S3 Best Practices](https://docs.aws.amazon.com/AmazonS3/latest/userguide/best-practices.html)
