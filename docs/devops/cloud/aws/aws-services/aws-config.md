# AWS Config

AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. It
provides a detailed view of configuration history, compliance against rules, and relationships between resources.

This guide provides an overview of AWS Config, including how to enable it using the **AWS Console (UI)** and how to
interact with it programmatically using the **AWS SDKs and CLI**.

---

## Key Concepts

- **Configuration Recorder**: Captures resource configurations whenever they change.
- **Delivery Channel**: Delivers configuration snapshots and history to Amazon S3 and sends notifications to Amazon SNS.
- **Config Rules**: Predefined or custom rules that check whether resources comply with desired configurations.
- **Compliance Status**: Indicates whether a resource complies or is non-compliant with specific rules.

---

## Using AWS Console (UI)

### Enable AWS Config

1. Open the **AWS Management Console**.
2. Navigate to **AWS Config**.
3. Click **Set up AWS Config**.
4. Choose:
    - **Resources to record** (all supported resources, or specific ones).
    - **AWS Config role** (select an existing role or create a new service-linked role).
    - **Delivery channel**:
        - S3 bucket (for storing configuration history and snapshots).
        - SNS topic (optional, for compliance notifications).
5. Click **Confirm** to start recording configurations.

### View Resource Configurations

1. In the AWS Config console, go to **Resources**.
2. Search for a resource (e.g., EC2 instance, S3 bucket).
3. Click the resource to view:
    - **Current configuration**
    - **Configuration timeline**
    - **Compliance status**

### Add AWS Managed Rules

1. Go to **Rules** in the AWS Config console.
2. Click **Add rule**.
3. Choose a **managed rule** (e.g., s3-bucket-public-read-prohibited).
4. Provide a rule name and scope (specific resource type or tag).
5. Save and evaluate compliance.

---

## Developer (SDK/CLI) Usage

### Python (boto3) Example: Enable Recorder

```python
import boto3

config = boto3.client('config')

# Create configuration recorder
config.put_configuration_recorder(
    ConfigurationRecorder={
        'name': 'default',
        'roleARN': 'arn:aws:iam::123456789012:role/AWSConfigRole',
        'recordingGroup': {
            'allSupported': True
        }
    }
)

# Create delivery channel
config.put_delivery_channel(
    DeliveryChannel={
        'name': 'default',
        's3BucketName': 'my-config-bucket',
        'snsTopicARN': 'arn:aws:sns:us-east-1:123456789012:ConfigTopic'
    }
)

# Start recording
config.start_configuration_recorder(ConfigurationRecorderName='default')
```

### Python: Get Compliance Summary

```python
response = config.get_compliance_summary_by_config_rule()
print(response)
```

### AWS CLI Examples
```bash
# Start configuration recorder
aws configservice start-configuration-recorder \
    --configuration-recorder-name default

# Describe configuration recorders
aws configservice describe-configuration-recorders

# List compliance by rule
aws configservice get-compliance-summary-by-config-rule

# Put a managed rule
aws configservice put-config-rule \
  --config-rule '{
      "ConfigRuleName": "s3-bucket-public-read-prohibited",
      "Source": {
        "Owner": "AWS",
        "SourceIdentifier": "S3_BUCKET_PUBLIC_READ_PROHIBITED"
      }
    }'
```

---

## Best Practices

- **Centralize Config**: Aggregate compliance data across accounts and regions using **AWS Config Aggregator**.
- **Automate Remediation**: Integrate with **AWS Systems Manager (SSM) Automation** for automatic remediation of
  non-compliant resources.
- **Secure Storage**: Use dedicated S3 buckets with versioning and encryption for Config snapshots.
- **Notifications**: Use SNS to receive real-time compliance updates.
- **Least Privilege**: Use minimal IAM permissions for Config roles.

---

## Common Use Cases

- **Security & Compliance Auditing**
    - Check if security groups allow unrestricted inbound access.
    - Verify if S3 buckets block public access.
- **Governance**
    - Ensure IAM policies follow organizational standards.
    - Verify EC2 instances are using approved AMIs.
- **Change Tracking**
    - Maintain historical configuration for troubleshooting.
- **Multi-Account Visibility**
    - Aggregate compliance data across AWS Organizations.

---

## Summary

AWS Config helps developers and organizations **monitor, audit, and enforce compliance** of AWS resources. You can use:

- **AWS Console (UI)** for quick setup and visualization.
- **AWS SDKs and CLI** for automation and integration into workflows.

By combining **Config Rules, Aggregators, and Automation**, you can build a scalable governance and compliance framework
across multiple AWS accounts and regions.
