# AWS Directory Service

AWS Directory Service provides managed directories in the cloud for your AWS resources. It allows you to run **Microsoft
Active Directory** (AD) or connect to an existing on-premises directory. Developers and administrators can use Directory
Service for authentication, resource management, and access control.

---

## Key Directory Types

- **AWS Managed Microsoft AD**: Fully managed Microsoft Active Directory hosted in AWS.
- **Simple AD**: Lightweight AD-compatible directory for small workloads.
- **AD Connector**: Proxy to connect AWS services to your on-premises Active Directory.
- **Amazon Cloud Directory**: A multi-dimensional directory for hierarchical data (used for application-specific
  directories).

---

## Using AWS Console (UI)

### Create an AWS Managed Microsoft AD

1. Open the **AWS Management Console** → **Directory Service**.
2. Click **Set up directory** → **AWS Managed Microsoft AD**.
3. Provide:
    - Directory name (e.g., `corp.example.com`)
    - Short name (e.g., `CORP`)
    - Admin password
    - VPC and subnets for directory servers
4. Review settings and click **Create Directory**.
5. Wait until status shows **Active**.

### Create an AD Connector

1. In Directory Service console, click **Set up directory** → **AD Connector**.
2. Provide:
    - Directory name
    - Connector description
    - On-premises AD DNS and NetBIOS name
    - VPC/subnets
    - Service account credentials
3. Click **Create Directory**.

### Join EC2 Instances to Directory

1. Launch an EC2 instance in the same VPC as the directory.
2. Under **System Properties → Computer Name → Change → Domain**, join the domain.
3. Provide directory admin credentials.

---

## Developer (SDK/CLI) Usage

### List Directories

```bash
aws ds describe-directories
```

### Create a Simple AD Directory

```bash
aws ds create-directory \
  --name "example.com" \
  --short-name "EXAMPLE" \
  --password "SuperSecretPassw0rd!" \
  --size Small \
  --vpc-settings SubnetIds=subnet-abc123,subnet-def456,VpcId=vpc-123abc
```

### Create an AD Connector

```bash
aws ds create-directory \
  --name "corp.example.com" \
  --short-name "CORP" \
  --password "ServiceAccountPassw0rd" \
  --size Small \
  --connect-settings "CustomerDnsIps=[10.0.0.10,10.0.0.11],CustomerUserName=ServiceAccount,SubnetIds=[subnet-abc123,subnet-def456],VpcId=vpc-123abc"
```

### Describe Directory

```bash
aws ds describe-directories --directory-ids d-1234567890
```

---

## Integration Examples

### Authenticate IAM Users via Directory

- Use **AWS Single Sign-On (SSO)** or integrate EC2 instances with AD for LDAP authentication.
- Applications can use LDAP/AD queries for access control.

### Manage EC2 Windows Instances

- Use **Group Policies** from AWS Managed AD.
- Apply domain-based user policies and software deployment.

---

## Best Practices

- **VPC Configuration**: Ensure directories are deployed in multiple subnets across Availability Zones for high
  availability.
- **DNS Resolution**: EC2 instances must use the directory DNS IPs for proper domain join.
- **IAM Integration**: Grant minimum privileges to AWS accounts or roles managing the directory.
- **Monitor Health**: Use CloudWatch metrics to monitor directory health, authentication failures, and replication.
- **Backup & Recovery**: AWS Managed Microsoft AD automatically replicates; still consider snapshots for additional
  protection.

---

## Common Use Cases

- **Enterprise User Management**: Centralize user accounts and authentication in the cloud.
- **Single Sign-On (SSO)**: Enable SSO to AWS applications and cloud resources.
- **Hybrid Environments**: Extend on-premises Active Directory to AWS for seamless identity management.
- **Windows Workloads**: Join EC2 Windows instances to managed domains for group policies and authentication.

---

## Summary

AWS Directory Service simplifies **identity and access management** in the cloud. Developers and administrators can:

- Use **AWS Console** for quick setup of managed directories.
- Use **AWS CLI/SDKs** for automation of directory creation and management.
- Integrate directories with **EC2 instances, applications, and IAM** to enforce authentication and policies.

AWS Directory Service ensures **secure, highly available, and scalable directory infrastructure** for cloud-based and
hybrid workloads.
