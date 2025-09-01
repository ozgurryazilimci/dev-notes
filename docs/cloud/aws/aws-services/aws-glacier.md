# AWS Glacier

## What is Amazon S3 Glacier?

Amazon S3 Glacier is a low-cost cloud storage service for data archiving and long-term backup. It is optimized for data
that is infrequently accessed and for which retrieval times of minutes to hours are acceptable.

Key features:

- Extremely low storage cost compared to S3 Standard
- Designed for long-term retention (years/decades)
- Retrieval options:
    - **Expedited** (1–5 minutes, higher cost)
    - **Standard** (3–5 hours)
    - **Bulk** (5–12 hours, lowest cost)
- Use cases:
    - Compliance and regulatory archives
    - Backups
    - Digital preservation

---

## Creating a Glacier Vault

### Using AWS Console (GUI)

1. Go to **AWS Management Console → S3 Glacier**.
2. Click **Create Vault**.
3. Choose a region.
4. Enter a vault name (e.g., `myvault`).
5. Optionally configure notifications or tags.
6. Click **Create Vault**.

### Using AWS CLI

```bash
aws glacier create-vault --account-id - --vault-name myvault
```

---

## Uploading Data to Glacier

Unlike S3 Standard storage, Glacier does not allow direct object upload from the console. You must use:

- **AWS CLI**
- **AWS SDKs**
- **Lifecycle policies** (transitioning S3 objects into Glacier)

---

## Example: Multipart Upload with AWS CLI

### 1. Create a Large File (3 MB)

```bash
dd if=/dev/urandom of=largefile bs=3145728 count=1
```

### 2. Split File into 1 MB Chunks

```bash
split --bytes=1048576 --verbose largefile chunk
```

Produces: `chunkaa`, `chunkab`, `chunkac`.

### 3. Initiate Multipart Upload

```bash
aws glacier initiate-multipart-upload --account-id - --archive-description "multipart upload test" --part-size 1048576
--vault-name myvault
```

Response:

```json
{
  "location": "...",
  "uploadId": "ssW0Kx..."
}
```

### 4. Assign Upload ID and Upload Parts

```bash
UPLOADID="ssW0Kx..."
```

```bash
aws glacier upload-multipart-part
--upload-id $UPLOADID --body chunkaa --range 'bytes 0-1048575/*' --account-id - --vault-name myvault  

aws glacier upload-multipart-part --upload-id $UPLOADID --body chunkab --range 'bytes 1048576-2097151/*' --account-id -
--vault-name myvault  

aws glacier upload-multipart-part --upload-id $UPLOADID --body chunkac --range 'bytes 2097152-3145727/*' --account-id -
--vault-name myvault
```

### 5. Generate Checksums for Each Part

```bash
openssl dgst -sha256 -binary chunkaa > hash1  
openssl dgst -sha256 -binary chunkab > hash2  
openssl dgst -sha256 -binary chunkac > hash3
```

### 6. Combine Hashes

```bash
cat hash1 hash2 > hash12  
openssl dgst -sha256 -binary hash12 > hash12hash
```

```bash
cat hash12hash hash3 > hash123  
openssl dgst -sha256 hash123
```

Result:

```plaintext
SHA256(hash123)= 9628195f...
```

Assign to variable:

```bash
TREEHASH=9628195f...
```

### 7. Complete Multipart Upload

```bash
aws glacier complete-multipart-upload --checksum $TREEHASH --archive-size 3145728 --upload-id $UPLOADID --account-id -
--vault-name myvault
```

---

## Retrieving Data from Glacier

Data in Glacier cannot be instantly retrieved. You must initiate a retrieval job.

Example:

```bash
aws glacier initiate-job --account-id - --vault-name myvault --job-parameters '{"Type": "archive-retrieval", "
ArchiveId": "ARCHIVE_ID"}'
```

Then check job status:

```bash
aws glacier describe-job --account-id - --vault-name myvault --job-id JOB_ID
```

Once completed, download the archive:

```bash
aws glacier get-job-output --account-id - --vault-name myvault --job-id JOB_ID output_file.txt
```

---

## Lifecycle Policies with S3 + Glacier

Often, instead of directly uploading to Glacier, organizations:

- Store objects in **S3 Standard** first
- Define a **lifecycle policy** to transition them into **S3 Glacier** or **S3 Glacier Deep Archive** after a number of
  days

Example use case:

- Keep logs in S3 Standard for 30 days
- Move them to Glacier for 1 year
- Finally, move them to Glacier Deep Archive for long-term retention

---

## Additional Notes

- **Glacier Vault Lock**: Provides compliance controls (e.g., write-once-read-many, WORM).
- **Pricing**: Very cheap storage, but retrieval and early deletion (less than 90 days) have additional costs.
- **Alternatives**: For infrequently accessed but still “hot” data, consider **S3 Glacier Instant Retrieval** or **S3
  Glacier Flexible Retrieval** storage classes instead of pure Glacier vaults.

---

## References

- [Using Amazon S3 Glacier in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-glacier.html)
