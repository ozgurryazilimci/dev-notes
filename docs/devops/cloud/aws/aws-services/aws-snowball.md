# AWS Snowball

AWS Snowball is a petabyte-scale data transport solution that uses physical devices to transfer large amounts of data
into and out of AWS. It is ideal for migrating large datasets, backing up data, or transferring data from disconnected
environments.

---

## Key Concepts

- **Snowball Device**: Physical device provided by AWS for secure data transfer.
- **Snowball Edge**: Advanced device with compute capabilities (EC2, Lambda) and storage.
- **Job**: Represents a data transfer request to/from AWS.
- **Cluster**: Group of Snowball Edge devices for larger workloads.
- **Data Encryption**: All data is automatically encrypted with 256-bit keys managed by AWS KMS.

---

## Using AWS Console (UI)

### Create a Snowball Job

1. Open **AWS Management Console** → **Snow Family**.
2. Click **Create job**.
3. Choose job type:
    - **Import into AWS** (send data to AWS)
    - **Export from AWS** (retrieve data from AWS)
4. Configure:
    - **Job name**
    - **S3 bucket** or **Glacier vault**
    - **Device type** (Snowball or Snowball Edge)
    - **Shipping address**
5. Review and create the job.
6. AWS will ship the device to the specified address.

### Load Data on the Device

1. Connect the Snowball device to your local network.
2. Install the **Snowball Client**.
3. Unlock the device using the manifest and unlock code provided by AWS.
4. Transfer data to the device using CLI commands.

### Return Device

1. Disconnect the device.
2. Use the provided shipping label to return the device to AWS.
3. AWS imports the data into the target bucket or service.

---

## Developer (CLI/SDK) Usage

### Install Snowball Client

- The Snowball Client is available for Linux, Windows, and macOS.
- Download and configure according to the documentation.

### List Snowball Jobs

```bash
aws snowball list-jobs
```

### Describe a Job

```bash
aws snowball describe-job --job-id JID1234567890
```

### Create an Import Job (CLI)

```bash
aws snowball create-job \
  --job-type IMPORT \
  --resources '{"S3Resources":[{"BucketArn":"arn:aws:s3:::mybucket"}]}' \
  --address-id AD1234567890 \
  --shipping-option SECOND_DAY \
  --description "Data Migration Job"
```

---

## Snowball Edge (Compute & Storage)

- **Edge Compute**: Run EC2 instances or Lambda functions directly on the device.
- **Local Storage**: Up to 80 TB per device (varies by device type).
- **Use Cases**: Data collection in remote locations, IoT data aggregation, preprocessing before cloud import.

### Example: Run Lambda on Snowball Edge

1. Package Lambda function compatible with Snowball Edge runtime.
2. Deploy the function via AWS Console or CLI to the Snowball Edge device.
3. Trigger Lambda using local events (e.g., S3-like bucket events on the device).

---

## Best Practices

- **Encryption**: Always use AWS-managed KMS keys to encrypt your data.
- **Data Integrity**: Use checksums when copying data to verify successful transfer.
- **Device Security**: The Snowball device has tamper-evident seals and built-in encryption.
- **Network Preparation**: Ensure proper network configuration for large data transfers.
- **Job Planning**: Break large transfers into multiple jobs for faster parallel processing.
- **Monitoring**: Track job progress via AWS Console, CloudWatch, or CLI.

---

## Common Use Cases

- **Data Center Migration**: Move petabytes of data to AWS when network transfer is impractical.
- **Disaster Recovery**: Backup large datasets securely to S3 or Glacier.
- **Remote Data Collection**: Aggregate data from remote sites or offline locations.
- **Big Data Analytics**: Preload large datasets for analytics in AWS.

---

## Summary

AWS Snowball provides a **secure, efficient, and scalable** way to transfer large volumes of data to and from AWS.
Developers and IT teams can:

- Use **AWS Console** for job creation and monitoring.
- Use **AWS CLI/SDK** for automation and scripting.
- Leverage **Snowball Edge** for compute-enabled local processing.

Snowball is ideal for **large-scale migrations, offline environments, and hybrid cloud workflows**.
