# AWS CLI

This document provides installation steps, configuration, common AWS CLI commands, and advanced tips for working with
AWS services.

---

## Installation

- Using pip:

```bash
pip install awscli
```

- Using Homebrew (macOS):

```bash
brew install awscli
```

Check the installed version:

```bash
aws --version
```

---

## Configuration

Set up your AWS credentials and default settings:

```bash
aws configure
```

This will prompt you for:

- **AWS Access Key ID**
- **AWS Secret Access Key**
- **Default region name** (e.g., `eu-west-1`)
- **Default output format** (e.g., `json`)

For SSO configuration:

```bash
aws configure sso
```

---

## Common Commands

Clear cached credentials:

```bash
# Delete SSO cache
rm -rf ~/.aws/sso/cache
```

```bash
# Delete AWS CLI cache
rm -rf ~/.aws/cli/cache
```

Reconfigure AWS CLI:

```bash
aws configure
```

Configure with SSO:

```bash
aws configure sso
```

---

## Example AWS Config File (~/.aws/config)

```plaintext
[default]  
sso_start_url = https://d-12345678.awsapps.com/start/#/  
sso_region = eu-west-1  
sso_account_id = 123456789  
sso_role_name = aws_developer  
region = eu-west-1  
output = json
```

---

## SSO Login and Verification

Login using SSO:

```bash
aws sso login
```

List S3 buckets:

```bash
aws s3 ls
```

Check logged-in identity:

```bash
aws sts get-caller-identity  
aws sts get-caller-identity --profile default
```

---

## Notes

- If you get `aws config credentials` errors:
    - Make sure the config file has the correct values.
    - Clear caches (`~/.aws/sso/cache` and `~/.aws/cli/cache`).
    - Run `aws configure sso` again with defaults.

- In many cases, after `aws configure sso`, you can just run:

```bash
aws sso login  
aws s3 ls
```

---

## Helpful Commands

- General help:

```bash
aws help
```

- Service-specific help:

```bash
aws s3 help
```

- Command-specific help:

```bash
aws s3 ls help
```

- List contents of a specific bucket:

```bash
aws s3 ls s3://testbucket
```

---

## Command Pattern

The general AWS CLI command structure is:  
`aws <service> <command> [parameters]`

Example:

```bash
aws s3 ls
```

---

## Advanced Usage

### Profiles (Multi-Account Support)

You can manage multiple accounts by adding profiles in `~/.aws/config`:

```plaintext
[profile dev]  
region = eu-west-1  
output = json
```

```plaintext
[profile prod]  
region = eu-west-1  
output = json
```

Usage:

```bash
aws s3 ls --profile dev
aws s3 ls --profile prod
```

---

### Session Expiration

SSO sessions usually expire after 8–12 hours. To renew, simply run:

```bash
aws sso login
```

(No need to reconfigure.)

---

### MFA (Multi-Factor Authentication)

If you’re using IAM user credentials with MFA enabled, generate temporary session tokens:

```bash
aws sts get-session-token \\
--serial-number arn:aws:iam::123456789:mfa/username \\
--token-code 123456
```

---

### Output Formats

You can change the output format with `--output`:

- `json` (default)
- `table` (human-readable)
- `text` (script-friendly)

Example:

```bash
aws s3 ls --output table
```

---

### JMESPath Queries (--query)

Filter and format command output directly in the CLI.

Example: Get instance IDs from EC2:

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].InstanceId" --output text
```

Example: List only S3 bucket names:

```bash
aws s3api list-buckets --query "Buckets[].Name" --output table  
```
