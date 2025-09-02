# AWS Services - EC2

Amazon Elastic Compute Cloud (EC2) is a core AWS service that provides scalable virtual servers in the cloud. This guide
covers the AWS Management Console interface for EC2, explaining key features, configuration options, and usage scenarios
for developers.

---

## EC2 Dashboard Overview

When you open the EC2 service in the AWS Console, the dashboard provides:

- **Instances**: Running and stopped instances.
- **Instance Types**: VM hardware configurations.
- **Key Pairs**: SSH key pairs for login.
- **Security Groups**: Virtual firewalls for EC2 instances.
- **Elastic IPs**: Static IP addresses for instances.
- **Volumes**: Attached block storage (EBS).
- **Snapshots**: Backups of EBS volumes.
- **Load Balancers**: Traffic distribution across multiple instances.
- **Auto Scaling Groups**: Automated instance scaling.

---

## EC2 Basics

### Instance Types

EC2 offers different instance families optimized for specific workloads:

- **General Purpose (t, m families)**: Balanced compute, memory, and networking.
- **Compute Optimized (c family)**: High-performance processors for CPU-intensive workloads.
- **Memory Optimized (r, x, z families)**: Optimized for memory-intensive applications such as databases.
- **Storage Optimized (i, d families)**: High IOPS, designed for large datasets and storage-heavy tasks.
- **Accelerated Computing (p, g, f families)**: GPU and FPGA-powered workloads such as ML and HPC.

Reference: [EC2 Instances Info](https://instances.vantage.sh/)

### Amazon Machine Images (AMIs)

An **AMI** is a template that defines the software configuration of an instance.

- **Public AMIs**: Provided by AWS or the community.
- **Private AMIs**: Custom-built for internal use.
- **Marketplace AMIs**: Paid AMIs from third-party vendors.

When launching, you select:

- Base operating system (Amazon Linux, Ubuntu, Windows, etc.)
- Virtualization type (HVM, PV)
- Architecture (Intel, AMD, ARM)

---

## Launching an Instance

### Steps in the Console

1. **Choose AMI (Amazon Machine Image)**
    - Preconfigured OS + software image (e.g., Amazon Linux, Ubuntu, Windows Server).

2. **Choose Instance Type**
    - Defines vCPU, memory, storage, and networking.
    - Families:
        - `t` (General Purpose, burstable performance)
        - `m` (General Purpose, balanced)
        - `c` (Compute-optimized)
        - `r` (Memory-optimized)
        - `p/g/inf/trn` (Accelerated computing, GPUs, ML/AI)

3. **Configure Instance Details**
    - Network (VPC and subnet)
    - Auto-assign public IP
    - Placement group (for high-performance computing)
    - IAM role (assign permissions to instance)
    - Shutdown behavior (Stop vs Terminate)

4. **Add Storage**
    - **Root Volume**: Usually EBS.
    - **Additional Volumes**: EBS or instance store.
    - Choose storage type:
        - gp3 (General purpose SSD)
        - io1/io2 (Provisioned IOPS SSD)
        - st1 (Throughput-optimized HDD)
        - sc1 (Cold HDD)

5. **Add Tags**
    - Key-value pairs for identifying resources (e.g., `Name: WebServer`).

6. **Configure Security Group**
    - Define inbound/outbound rules (e.g., allow SSH `22/tcp`, HTTP `80/tcp`).

7. **Key Pair (Login)**
    - Required for SSH/RDP access.
    - `.pem` file for Linux/macOS SSH, `.ppk` for Windows PuTTY.

---

## Instance Management

- **Start/Stop/Terminate**: Basic lifecycle actions.
- **Connect to Instance**:
    - EC2 Instance Connect (browser-based SSH).
    - SSH via terminal.
    - Session Manager (no SSH key needed, secure).

- **Monitoring**:
    - CloudWatch metrics (CPU, disk, network).
    - Detailed monitoring option (1-minute granularity).

- **Status Checks**:
    - System reachability check.
    - Instance reachability check.

---

## Networking Features

- **Elastic IPs**: Static IPv4 addresses that can be reassigned.
- **Elastic Network Interfaces (ENIs)**: Additional network cards for multi-homed setups.
- **Security Groups**: Stateful firewalls at instance level.
- **Network ACLs**: Stateless firewalls at subnet level.
- **VPC Integration**: EC2 instances run inside a VPC subnet.

---

## Storage Options

- **EBS (Elastic Block Store)**:
    - Persistent block storage.
    - Snapshots for backup and restore.

- **Instance Store**:
    - Temporary local storage (data lost on stop/terminate).

- **EFS (Elastic File System)**:
    - Shared NFS file system for multiple instances.

- **FSx**:
    - Managed Windows File Server or Lustre file systems.

### Snapshots

- Point-in-time backup of volumes.
- Snapshots are incremental and can be copied across regions.
- Used to create new AMIs or restore data.

### RAID Configurations

For high availability and performance:

- RAID 0 (striping, performance, no redundancy).
- RAID 1 (mirroring, redundancy).
- RAID 5 (striping with parity, requires at least 3 volumes).

---

## Elastic Load Balancing (ELB)

Distributes traffic across multiple instances:

- **Classic Load Balancer (CLB)**: Basic, L4/L7 load balancing.
- **Application Load Balancer (ALB)**: Layer 7 (HTTP/HTTPS), microservices, containers.
- **Network Load Balancer (NLB)**: Layer 4 (TCP/UDP), ultra-low latency.
- **Gateway Load Balancer (GWLB)**: For firewalls/proxies

Key settings:

- Listeners (protocol/port).
- Target groups (instances, IPs, Lambda).
- Health checks (ensure only healthy targets receive traffic).

---

## Auto Scaling

Automatically scale instance count based on demand.

- **Launch Template / Configuration**: Defines instance setup.
- **Auto Scaling Group (ASG)**:
    - Automatically add/remove instances based on demand.
    - Supports scaling policies (CPU, network usage).
    - Minimum, maximum, desired instance count.
    - Integrated with ELB for high availability.
- **Scaling Policies**:
    - Simple (e.g., CPU > 70% → add instance, CPU < 30% → remove instance).
    - Target Tracking (keep metric at a target value).
    - Scheduled (scale at specific times).

---

## Security Features

- **IAM Roles**: Assign AWS permissions to instances securely.
- **Key Pairs**: Authentication method for login.
- **Security Groups & NACLs**: Control traffic at instance and subnet levels.
- **Encryption**:
    - EBS volume encryption.
    - Data in transit via TLS/SSL.

---

## Key Pairs and Connectivity

### Linux/Mac

```bash
chmod 400 mykey.pem
ssh -i mykey.pem ec2-user@<PUBLIC_IP>
```

### Windows (PuTTY)

1. Convert `.pem` to `.ppk` using PuTTYgen.
2. Configure host: `ec2-user@<PUBLIC_IP>`.
3. Authentication: Load `.ppk` private key.

---

## Advanced Features

- **Elastic GPU**: Attach GPU resources for graphics.
- **Dedicated Hosts**: Physical servers for license compliance.
- **Placement Groups**:
    - Cluster: Low-latency HPC.
    - Spread: Instances across hardware for resilience.
    - Partition: Large distributed workloads.

- **Spot Instances**: Low-cost instances using spare capacity (can be interrupted).
- **Reserved Instances**: Discounted pricing for 1- or 3-year commitment.
- **Savings Plans**: Flexible pricing model compared to Reserved Instances.

---

## Backups & Recovery

- **EBS Snapshots**: Backup volumes to S3.
- **AMI Creation**: Create custom reusable machine images.
- **Recovery Options**: Replace root volume, recover from snapshots.

---

## Monitoring & Logging

- **CloudWatch Metrics**: CPU, disk, network, status checks.
- **CloudWatch Logs**: Send application logs from instance.
- **VPC Flow Logs**: Monitor network traffic.
- **CloudTrail**: Track API calls related to EC2.

---

## Pricing Considerations

- **On-Demand**: Pay per second/hour.
- **Reserved Instances**: 1–3 year commitment for discounts.
- **Spot Instances**: 70–90% cheaper, but interruptible.
- **Savings Plans**: Flexible commitment model.

---

## Common Use Cases

- Hosting web applications.
- Running databases (with proper tuning).
- Batch processing and data analysis.
- High-performance computing (HPC).
- Machine learning and GPU workloads.

---

## Best Practices

- Use IAM roles instead of storing credentials on EC2.
- Keep OS and software patched via user data or automation.
- Use Security Groups with least privilege (avoid 0.0.0.0/0 when possible).
- Encrypt EBS volumes and snapshots.
- Distribute workloads across Availability Zones for high availability.
- Monitor with CloudWatch and set alarms.
- Use Auto Scaling with Load Balancers for production workloads.

---

## Demo

### Auto Scaling Demo: Stress Test

To validate that Auto Scaling responds correctly to load, you can generate artificial CPU stress on an EC2 instance and
observe how the Auto Scaling Group launches new instances.

#### Steps

- **Create an Auto Scaling Group**
    - Launch Template or Launch Configuration with your EC2 setup.
    - Attach to a Target Group (behind a Load Balancer for best results).
    - Define scaling policies:
        - Scale out: Add instances when **Average CPU > 70% for 5 minutes**.
        - Scale in: Remove instances when **Average CPU < 30% for 5 minutes**.

- **SSH into one of the EC2 instances** and install `stress`:

```bash
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm -y
yum install stress -y
```

- **Run the stress test** to simulate high CPU load:

```bash
stress --cpu 80 --timeout 2000
```

> This command spawns 80 CPU workers and runs them for 2000 seconds.
