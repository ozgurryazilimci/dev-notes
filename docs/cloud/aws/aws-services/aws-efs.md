# AWS Services - EFS (Elastic File System)

Amazon Elastic File System (EFS) is a fully managed, serverless, elastic file storage service that can be mounted
concurrently on multiple EC2 instances, containers, and AWS services. It is highly available and scales automatically as
files are added or removed.

![](<images/aws_efs.png>)

---

## Overview of EFS

- **Elastic and Scalable**: Storage automatically grows and shrinks as you add or remove data, no provisioning needed.
- **Shared File System**: Multiple EC2 instances or containers can mount the same file system concurrently.
- **Durability and Availability**: Data is stored redundantly across multiple Availability Zones.
- **POSIX-Compliant**: Works like a standard Linux file system, supporting typical file system semantics.
- **Use Cases**: Web serving, content management, container storage (EKS/ECS), data analytics, shared developer
  environments.

---

## Storage Classes

EFS provides lifecycle management and cost optimization through different storage classes:

- **EFS Standard**: High availability and durability, frequently accessed files.
- **EFS Standard-IA (Infrequent Access)**: Lower cost for less frequently accessed data.
- **EFS One Zone**: Stores data in a single AZ (cheaper, but less redundant).
- **EFS One Zone-IA**: Combination of single-AZ and infrequent access.

Lifecycle policies can automatically move files between Standard and IA tiers.

---

## Performance Modes

When creating an EFS, you choose a **Performance Mode**:

- **General Purpose**: Default, suitable for most applications.
- **Max I/O**: For workloads with highly parallelized access (e.g., thousands of EC2 instances).

---

## Throughput Modes

Throughput can scale automatically with storage or be provisioned:

- **Bursting Throughput**: Default, scales with storage size.
- **Provisioned Throughput**: Fixed throughput regardless of storage size, useful for small but high-IO workloads.

---

## Creating an EFS via Console

Steps in the AWS Console:

1. Go to **EFS > Create file system**.
2. Choose **VPC and subnets** (EFS will create mount targets in selected subnets).
3. Configure **security groups** to allow NFS (port 2049).
4. Select **Performance mode** and **Throughput mode**.
5. Enable **Lifecycle management** if cost optimization is needed.
6. Add **tags** for identification.
7. Create the file system.

---

## Mounting EFS on EC2

### Install NFS Utilities

On Amazon Linux or similar:

```bash
sudo yum install -y amazon-efs-utils  
sudo yum install -y nfs-utils
```

### Mount via EFS Mount Helper

```bash
sudo mkdir /mnt/efs  
sudo mount -t efs fs-12345678:/ /mnt/efs
```

### Mount via NFS Client

```bash
sudo mount -t nfs4 -o nfsvers=4.1 fs-12345678.efs.eu-west-1.amazonaws.com:/ /mnt/efs
```

To make it persistent, add to `/etc/fstab`.

---

## Access and Security

- **Security Groups**: Allow inbound traffic on port 2049 (NFS).
- **Network**: Mount targets exist in each subnet/AZ, ensure instances are in same VPC.
- **IAM Policies**: Used with access points for fine-grained permissions.
- **EFS Access Points**: Simplify access for multiple users/applications by providing unique entry points with enforced
  user ID and root directory.

---

## Backup and Monitoring

- **AWS Backup**: Fully supported for automated backups of EFS file systems.
- **Monitoring**:
    - Use CloudWatch for metrics (IOPS, throughput, data size).
    - Use CloudTrail to log API calls.

---

## Example Use Cases

- **Web Content Hosting**: Multiple EC2s behind a Load Balancer can share the same EFS.
- **Containerized Workloads**: Persistent storage for ECS/EKS pods.
- **Data Science & ML**: Central dataset accessible by multiple compute nodes.
- **CI/CD Pipelines**: Shared build artifacts across multiple environments.

---

## Best Practices

- Use **lifecycle management** to reduce costs for infrequently accessed files.
- Place EC2 instances and EFS mount targets in the same VPC and AZ for low latency.
- Use **Access Points** for multi-tenant environments.
- Enable **encryption at rest and in transit**.
- Monitor with CloudWatch and set alarms for throughput or burst credits.
