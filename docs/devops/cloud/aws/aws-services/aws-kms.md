# AWS Key Management Service (KMS)

AWS Key Management Service (KMS) is a fully managed service that makes it easy to create, control, and manage
cryptographic keys used to protect data. Developers can use KMS to encrypt/decrypt data, sign/verify messages, and
control access to keys using IAM policies.

---

## Key Concepts

- **Customer Master Keys (CMKs)** / **KMS Keys**: The primary resource in KMS used for encryption and signing.
- **Symmetric Keys**: A single key used for both encryption and decryption (AES-256).
- **Asymmetric Keys**: Key pairs (RSA, ECC) for encryption/decryption or signing/verification.
- **Automatic Key Rotation**: Rotate keys every year for enhanced security.
- **Grants**: Permissions to use a KMS key without changing IAM policies.
- **Envelope Encryption**: Best practice to use KMS keys to encrypt/decrypt **data keys**, not the raw data itself.

---

## Using AWS Console (UI)

### Create a KMS Key

1. Open **AWS Management Console** → **KMS**.
2. Click **Create key**.
3. Choose:
    - **Key type**: Symmetric or Asymmetric.
    - **Key usage**: Encrypt/Decrypt, Sign/Verify, Generate/Verify MAC.
4. Configure key settings:
    - Alias (e.g., `my-app-key`)
    - Description
    - Key administrators (IAM users/roles)
    - Key usage permissions
5. Review and **Create key**.

### Encrypt & Decrypt via Console

1. In **KMS Console**, select your key.
2. Use **Encrypt** tool to upload plaintext and get ciphertext.
3. Use **Decrypt** tool to reverse ciphertext back into plaintext (IAM permissions required).

---

## Developer (CLI/SDK) Usage

### List Keys

```bash
aws kms list-keys
```

### Create a Symmetric Key

```bash
aws kms create-key \
  --description "My application key" \
  --key-usage ENCRYPT_DECRYPT \
  --origin AWS_KMS
```

### Encrypt Data

```bash
aws kms encrypt \
  --key-id <key-id> \
  --plaintext "HelloWorld" \
  --query CiphertextBlob \
  --output text
```

### Decrypt Data

```bash
aws kms decrypt \
  --ciphertext-blob fileb://encrypted.txt \
  --output text \
  --query Plaintext \
  | base64 --decode
```

### Generate a Data Key (Envelope Encryption)

```bash
aws kms generate-data-key \
  --key-id <key-id> \
  --key-spec AES_256
```

This returns both:

- **Plaintext data key** (use it to encrypt application data).
- **Encrypted data key** (store it securely and use KMS to decrypt later).

---

## Python (boto3) Example

```python
import boto3, base64

kms = boto3.client('kms')

# Encrypt text
response = kms.encrypt(
    KeyId='alias/my-app-key',
    Plaintext=b'Hello Secure World'
)
ciphertext = response['CiphertextBlob']
print("Encrypted:", base64.b64encode(ciphertext).decode())

# Decrypt text
response = kms.decrypt(CiphertextBlob=ciphertext)
plaintext = response['Plaintext']
print("Decrypted:", plaintext.decode())
```

## Best Practices

- **Use Aliases**: Instead of hardcoding key IDs, use aliases for better management.
- **Least Privilege**: Limit IAM users/roles that can manage or use KMS keys.
- **Automatic Rotation**: Enable automatic key rotation for symmetric keys.
- **Audit Logging**: Enable **AWS CloudTrail** to log key usage.
- **Envelope Encryption**: Encrypt large data with data keys, not directly with CMKs.
- **Cross-Account Access**: Use grants or resource policies to share keys securely.

---

## Common Use Cases

- **S3 Object Encryption**: Encrypt objects with KMS-managed keys (SSE-KMS).
- **EBS Volume Encryption**: Encrypt EC2 volumes automatically with KMS.
- **Database Encryption**: Use KMS with RDS, DynamoDB, or Redshift.
- **Application Secrets**: Encrypt sensitive config data (e.g., passwords, API keys).
- **Code Signing**: Sign code artifacts with asymmetric KMS keys.

---

## Summary

AWS KMS provides **centralized key management** with tight integration across AWS services. Developers can:

- Manage keys securely via **AWS Console**.
- Automate encryption/decryption with **AWS CLI and SDKs**.
- Implement best practices such as **envelope encryption, least-privilege IAM, and rotation**.

KMS ensures **data confidentiality, integrity, and compliance** for applications running in AWS.
