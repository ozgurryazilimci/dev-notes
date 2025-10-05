# AWS Organizations

AWS Organizations helps you centrally manage multiple AWS accounts. It provides **policy-based management**,
consolidated billing, and hierarchical account structures, allowing developers and administrators to govern
multi-account environments efficiently.

---

## Key Concepts

- **Organization**: The root container for all your AWS accounts.
- **Root**: The top-level node in your organization.
- **Organizational Unit (OU)**: A group of accounts for applying policies.
- **Account**: An individual AWS account within the organization.
- **Service Control Policies (SCPs)**: Policies that define permissions for accounts or OUs.
- **Consolidated Billing**: Aggregate billing for all member accounts.

---

## Using AWS Console (UI)

### Create an Organization

1. Open **AWS Management Console** → **AWS Organizations**.
2. Click **Create Organization**.
3. Choose **All features** (recommended for full functionality).
4. Your root account becomes the **management account**.

### Create Organizational Units (OUs)

1. Navigate to **Organize accounts**.
2. Click **Create Organizational Unit**.
3. Provide a name (e.g., `Dev`, `Prod`) and parent root.
4. Move accounts into the OU as needed.

### Apply Service Control Policies (SCPs)

1. Go to **Policies → Create policy**.
2. Define JSON rules to allow or deny specific AWS actions.
3. Attach the SCP to an **OU** or individual account.
4. Policies automatically restrict permissions for accounts under the OU.

### Invite Existing Accounts

1. Click **Accounts → Invite account**.
2. Provide the **account ID or email**.
3. The invited account owner must accept the invitation to join.

---

## Developer (CLI/SDK) Usage

### List Accounts

```bash
aws organizations list-accounts
```

### Create an Organizational Unit

```bash
aws organizations create-organizational-unit \
  --parent-id r-examplerootid123 \
  --name Dev
```

### Attach SCP to an OU

```bash
aws organizations attach-policy \
  --policy-id p-examplepolicyid123 \
  --target-id ou-examplerootid123-dev
```

### Create a Policy

```bash
aws organizations create-policy \
  --content file://deny-s3-public.json \
  --description "Deny public S3 access" \
  --name DenyPublicS3 \
  --type SERVICE_CONTROL_POLICY
```

### Invite Account

```bash
aws organizations invite-account-to-organization \
  --target Id=123456789012,Type=ACCOUNT \
  --notes "Join our organization"
```

---

## Integration Examples

- **Centralized Billing**
    - Consolidate billing for multiple accounts under one management account.
- **Access Governance**
    - Apply SCPs to OUs to enforce security and compliance standards.
- **Automated Account Provisioning**
    - Use CLI/SDK to programmatically create new accounts in an OU.
- **Cross-Account Resource Management**
    - Use Organizations combined with AWS IAM Roles for cross-account access.

---

## Best Practices

- **Use OUs Strategically**: Group accounts by environment (Prod, Dev, Test) or business unit.
- **Least Privilege via SCPs**: Restrict sensitive services (e.g., EC2, IAM) for lower-risk accounts.
- **Centralized Billing**: Monitor cost and usage reports from the management account.
- **Automation**: Script account creation and SCP attachments for consistency.
- **Audit & Compliance**: Enable AWS CloudTrail across all accounts for activity monitoring.
- **Service Control Policies (SCPs)**: Start with broad policies at root, refine for individual OUs.

---

## Common Use Cases

- **Multi-Account Management**
    - Organize accounts for development, staging, and production environments.
- **Governance & Compliance**
    - Enforce policies to prevent non-compliant resource usage.
- **Billing Consolidation**
    - Aggregate usage and spend from all accounts to reduce costs.
- **Account Lifecycle Automation**
    - Automate onboarding/offboarding of AWS accounts using scripts or CloudFormation.

---

## Summary

AWS Organizations provides **centralized governance and account management** across multiple AWS accounts. Developers
and administrators can:

- Use **AWS Console** for quick setup of organization, OUs, and policies.
- Automate account creation, policy attachment, and monitoring using **AWS CLI and SDKs**.
- Ensure **compliance, security, and cost management** across all accounts.

By combining **SCPs, OUs, and consolidated billing**, Organizations enables scalable and secure multi-account
environments.
