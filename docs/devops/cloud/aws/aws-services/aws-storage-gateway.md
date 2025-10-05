# AWS Storage Gateway

AWS Storage Gateway is a hybrid cloud storage service that connects on-premises environments with AWS cloud storage. It
provides seamless integration with Amazon S3, EBS, and Glacier, allowing developers to use cloud storage for backup,
disaster recovery, and file sharing while maintaining low-latency access locally.

---

## Key Storage Gateway Types

- **File Gateway**: Presents S3 buckets as NFS or SMB file shares for local applications.
- **Volume Gateway**: Provides block storage that can be presented as iSCSI volumes, either:
    - **Cached Volumes**: Store frequently accessed data locally; the rest in S3.
    - **Stored Volumes**: Keep primary data on-premises with asynchronous cloud backups.
- **Tape Gateway**: Emulates a virtual tape library (VTL) for backup software, storing virtual tapes in Glacier or S3.

---

## Using AWS Console (UI)

### Deploy a Storage Gateway

1. Open the **AWS Management Console** → **Storage Gateway**.
2. Click **Create gateway**.
3. Choose gateway type (File, Volume, or Tape).
4. Select activation method:
    - **On-premises VM** (VMware ESXi, Hyper-V, or KVM)
    - **AWS EC2 instance** for gateway in the cloud
5. Download and deploy the virtual appliance (if on-premises).
6. Activate the gateway using the provided activation key.

### Configure a File Gateway

1. Create a **File Share**.
2. Choose protocol (NFS or SMB).
3. Associate with an **S3 bucket**.
4. Set access control, encryption, and caching options.
5. Mount the file share from your local application or server.

### Configure a Volume Gateway

1. Create **iSCSI volumes**.
2. Choose volume type: **Cached** or **Stored**.
3. Configure local cache size, backup frequency, and snapshot options.
4. Connect to on-premises applications via iSCSI initiators.

---

## Developer (CLI/SDK) Usage

### List Gateways

```bash
aws storagegateway list-gateways
```

### Describe Gateway

```bash
aws storagegateway describe-gateway-information \
  --gateway-arn arn:aws:storagegateway:us-east-1:123456789012:gateway/sgw-12A3456B
```

### Create File Share (CLI)

```bash
aws storagegateway create-nfs-file-share \
  --gateway-arn arn:aws:storagegateway:us-east-1:123456789012:gateway/sgw-12A3456B \
  --role arn:aws:iam::123456789012:role/StorageGatewayRole \
  --location-arn arn:aws:s3:::my-bucket \
  --client-list 10.0.0.0/24
```

### Create Volume (CLI)

```bash
aws storagegateway create-cached-iscsi-volume \
  --gateway-arn arn:aws:storagegateway:us-east-1:123456789012:gateway/sgw-12A3456B \
  --volume-size-in-bytes 107374182400 \
  --snapshot-id snap-0abcdef1234567890 \
  --target-name myVolumeTarget
```

---

## Integration Examples

- **Backup and Restore**
    - Use Tape Gateway to emulate virtual tapes and store in Glacier.
- **Hybrid Cloud File Access**
    - Use File Gateway to provide applications local access to S3-backed files.
- **Disaster Recovery**
    - Use Volume Gateway with snapshots for quick recovery to AWS.

---

## Best Practices

- **Network Configuration**: Ensure low-latency, high-bandwidth connectivity for optimal performance.
- **Caching**: Allocate sufficient local cache for File and Volume Gateways to reduce latency.
- **IAM Roles**: Grant only necessary permissions to the gateway for S3, EBS, or Glacier access.
- **Monitoring**: Use CloudWatch metrics and logs for performance and health monitoring.
- **Data Security**: Enable encryption at rest (S3/KMS) and in transit (SMB/NFS).

---

## Common Use Cases

- **Hybrid File Storage**: Local file access with seamless cloud backup.
- **Disaster Recovery**: Store snapshots and backups in AWS for fast recovery.
- **Tape Replacement**: Use virtual tapes for legacy backup applications.
- **Cloud Data Migration**: Move large datasets to S3 with minimal disruption to on-premises apps.

---

## Summary

AWS Storage Gateway enables **hybrid cloud storage solutions** for organizations needing seamless integration of
on-premises systems with AWS cloud storage. Developers and administrators can:

- Configure **File, Volume, or Tape gateways** through **AWS Console**.
- Automate setup and management using **AWS CLI/SDK**.
- Implement **secure, scalable, and highly available storage architectures** for backup, disaster recovery, and hybrid
  cloud use cases.
