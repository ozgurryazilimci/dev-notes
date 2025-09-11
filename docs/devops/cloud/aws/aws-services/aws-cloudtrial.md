# AWS Services - CloudTrail

## Overview

**AWS CloudTrail** is a service that enables governance, compliance, and operational and risk auditing of your AWS
account.  
With CloudTrail, you can log, continuously monitor, and retain account activity related to actions across your AWS
infrastructure.

CloudTrail records **API calls** made in your account, including:

- Actions taken through the AWS Management Console.
- API requests made via SDKs and AWS CLI.
- Resource changes triggered by AWS services.

---

## Key Concepts

- **Events**: Every API call in AWS generates an event. CloudTrail records the request, source IP, user identity,
  service, and parameters.
- **Event history**: Viewable in the console, stores last 90 days of account activity by default.
- **Trails**: Configuration that delivers CloudTrail events to a storage destination (S3, CloudWatch Logs, or
  EventBridge).
- **Management events**: Record management operations (e.g., creating/deleting EC2 instances).
- **Data events**: Record S3 object-level activity and Lambda function invocations.
- **Insight events**: Detect unusual operational activity (e.g., spikes in failed logins or API error rates).

---

## Why Use CloudTrail?

- **Security auditing**: Track who accessed what and when.
- **Compliance**: Maintain audit trails for regulations like PCI DSS, HIPAA, GDPR.
- **Forensics**: Investigate unauthorized or suspicious activity.
- **Operational troubleshooting**: Identify configuration changes leading to issues.
- **Monitoring**: Feed events into CloudWatch or EventBridge for real-time alerts.

---

## Setting Up CloudTrail

1. Go to **CloudTrail** in the AWS Management Console.
2. Choose **Create trail**.
3. Configure:
    - Trail name.
    - Apply to **all regions** (recommended).
    - Choose **Management events**, **Data events**, or both.
    - Select **S3 bucket** for log storage.
    - Optionally, enable **CloudWatch Logs** integration for real-time monitoring.
4. Save and enable the trail.

---

## Event Structure

A typical CloudTrail event is in JSON format and includes:

```json
{
  "eventTime": "2025-09-10T12:34:56Z",
  "eventSource": "ec2.amazonaws.com",
  "eventName": "StartInstances",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "203.0.113.45",
  "userAgent": "console.amazonaws.com",
  "requestParameters": {
    "instancesSet": {
      "items": [
        {
          "instanceId": "i-1234567890abcdef0"
        }
      ]
    }
  },
  "responseElements": null,
  "userIdentity": {
    "type": "IAMUser",
    "userName": "devuser"
  }
}
```

---

## CloudTrail with S3 and CloudWatch

- **S3**: All logs can be stored in a central S3 bucket for long-term retention.
- **CloudWatch Logs**: Send events to CloudWatch for real-time monitoring and alarms.
- **EventBridge**: Trigger workflows (e.g., Lambda functions) in response to events.

Example: Automatically disable compromised IAM credentials if suspicious activity is detected.

---

## Searching Event History

CloudTrail console provides a 90-day event history:

- Filter by **user name**, **event source**, **event name**, or **time range**.
- Useful for quick investigations.

For longer retention or complex analysis:

- Use **Athena** to query CloudTrail logs in S3 with SQL.
- Integrate with **AWS Lake Formation** or **Amazon OpenSearch** for advanced analytics.

---

## CloudTrail Insights

- Detects unusual activity in your account.
- Examples:
    - Sudden spike in `StartInstances` calls.
    - Higher-than-usual failed `ConsoleLogin` attempts.
- Helps in identifying operational anomalies and potential security breaches.

---

## Security Best Practices

- Always enable **multi-region trails** to capture events across all regions.
- Store CloudTrail logs in an **encrypted S3 bucket** with limited access.
- Enable **log file integrity validation** to detect tampering.
- Forward logs to **CloudWatch Logs** or **SIEM tools** for alerting.
- Combine with **AWS Config** for compliance monitoring.

---

## Example CLI Commands

Enable a new trail:

```bash
aws cloudtrail create-trail --name MyTrail --s3-bucket-name my-cloudtrail-logs
```

Start logging:

```bash
aws cloudtrail start-logging --name MyTrail
```

Check trails:

```bash
aws cloudtrail describe-trails
```

Lookup recent events:

```bash
aws cloudtrail lookup-events --max-results 5
```

---

## Summary

- **CloudTrail** records all AWS API activity for auditing and compliance.
- Events can be viewed in the console, stored in S3, analyzed with Athena, or monitored with CloudWatch.
- **Trails** provide centralized storage and monitoring of events across regions.
- **Insights** highlight unusual activity patterns for proactive response.
- Essential for **security, compliance, and operational monitoring** in any AWS environment.  
