# AWS Services - Identity and Access Management (IAM)

## Introduction

**AWS Identity and Access Management (IAM)** is a global service that allows you to securely manage access to AWS
services and resources.  
With IAM, you can create and manage **users**, **groups**, and **roles**, and use **policies** to define permissions.

---

## Root User & Security Best Practices

- The **root user** is created when you first set up your AWS account.
- It has **full access** to all services and resources.
- Best practices:
    - Do **not** use the root account for everyday tasks.
    - Enable **Multi-Factor Authentication (MFA)** for the root user.
    - Create IAM users with least-privilege permissions instead.

---

## IAM Users and Groups

- **Users** represent individual identities with specific credentials.
- **Groups** are collections of users that share the same permissions.
- Example:
    - `Developers` group with access to EC2
    - `Admins` group with administrator privileges

You can assign users to groups or grant permissions directly.

---

## IAM Policies

IAM Policies define what actions are allowed or denied.  
They are written in **JSON** and attached to users, groups, or roles.

### Example: Administrator Access Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

### Key Elements:

- **Effect**: Allow or Deny
- **Action**: Specifies which API operations are allowed/denied
- **Resource**: The AWS resources the policy applies to

You can create policies using:

- **Policy Generator / Visual Editor**
- **JSON Editor** (custom fine-grained control)

### Custom Policies

If you need more fine-grained access:

- Create a **custom policy** in JSON or Visual Editor.
- Attach it to the user or group.

Example: Allow listing S3 buckets only.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    }
  ]
}
```

---

## IAM Roles

Roles are similar to users but are **not associated with a single person**.  
They are intended for AWS services or cross-account access.

Examples:

- **Lambda Role** → Lambda function assumes a role to access S3.
- **EC2 Role** → EC2 instance can access an S3 bucket without storing credentials.
- **Cross-account Role** → Allow a user from another AWS account to access your resources.

Each role is attached to a **policy** that defines permissions.

---

## Password Policies & Account Settings

- Enforce strong passwords for IAM users
- Configure account-wide password policy:
    - Minimum password length
    - Require uppercase/lowercase letters, numbers, special characters
    - Password expiration and reuse prevention

---

## Account Alias & Login URL

- You can set an **account alias** to create a user-friendly login URL.  
  Example:  
  Instead of `https://123456789012.signin.aws.amazon.com/console`  
  You can use: `https://mycompany.signin.aws.amazon.com/console`

---

## Access Analyzer & Credential Reports

- **Credential Reports**: Downloadable CSV report listing all users and their credentials status (passwords, keys, MFA,
  etc.)
- **IAM Access Analyzer**: Identifies resources (like S3 buckets, IAM roles, KMS keys) that are shared with external
  entities.

---

## Best Practices for IAM

- Always follow the **principle of least privilege**.
- Regularly rotate credentials and access keys.
- Enable **MFA** for all sensitive accounts.
- Use **roles instead of long-term credentials** for applications.
- Monitor IAM activity with **CloudTrail**.

---

## Summary

IAM is the backbone of AWS security.  
By managing **users, groups, roles, and policies**, enforcing **password/MFA requirements**, and monitoring with *
*reports and analyzers**, you can ensure secure and controlled access to AWS resources.
