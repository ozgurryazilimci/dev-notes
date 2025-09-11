# AWS Services - CloudWatch

## Overview

**Amazon CloudWatch** is a monitoring and observability service built into AWS.  
It provides metrics, logs, and alarms to help developers, DevOps engineers, and system administrators monitor resources,
applications, and services in real-time.

Use cases:

- Collecting and analyzing metrics from AWS services (EC2, RDS, Lambda, etc.).
- Centralized log collection with CloudWatch Logs.
- Setting up alarms and automated actions.
- Creating dashboards for visualization.
- Detecting anomalies and performance issues.

---

## Key Concepts

- **Metrics**: Time-ordered data points (e.g., CPU utilization, network traffic).
- **Logs**: Application and system logs collected and stored.
- **Alarms**: Rules to trigger actions based on metric thresholds.
- **Events (EventBridge)**: React to AWS resource changes in near real-time.
- **Dashboards**: Visual representation of metrics and logs.
- **Namespaces**: Categories for metrics (e.g., `AWS/EC2`, `AWS/RDS`).
- **Dimensions**: Key-value pairs to filter and identify metrics.

---

## CloudWatch Metrics

Metrics are collected from:

- **AWS services** (default, no setup required).
- **Custom metrics** (via API or CloudWatch Agent).
- **Applications/On-premises servers** (via CloudWatch Agent).

Examples:

- EC2: `CPUUtilization`, `DiskReadOps`
- RDS: `DatabaseConnections`, `FreeStorageSpace`
- Lambda: `Invocations`, `Duration`, `Errors`

---

## CloudWatch Logs

CloudWatch Logs allows you to:

- Collect logs from EC2, Lambda, or custom applications.
- Search, filter, and analyze logs.
- Set log retention policies.
- Export logs to S3 or stream to Lambda for further processing.

**Common use cases:**

- Application logs for debugging.
- System logs for security auditing.
- Custom logs for business metrics.

---

## CloudWatch Agent Installation (on EC2 or On-Prem)

To collect system-level metrics and logs, you install the **CloudWatch Agent**:

```bash
wget https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm
sudo rpm -U ./amazon-cloudwatch-agent.rpm
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```

- The **config wizard** helps create a JSON config for which metrics/logs to collect.
- Once configured, start the agent:

```bash
sudo systemctl start amazon-cloudwatch-agent
sudo systemctl enable amazon-cloudwatch-agent
```

---

## CloudWatch Alarms

- Alarms are created for specific metrics.
- Actions:
    - Send an **SNS notification** (email, SMS, webhook).
    - Trigger **Auto Scaling** policies.
    - Execute **EC2 recovery** or **stop/terminate actions**.

Example:  
Alarm on `CPUUtilization > 80% for 5 minutes` → send email via SNS.

---

## CloudWatch Dashboards

- Create custom dashboards in the console.
- Add widgets (line charts, bar charts, numbers).
- Combine multiple service metrics into one view.
- Useful for **NOC screens** or **real-time monitoring**.

---

## CloudWatch Events (Amazon EventBridge)

- Rules that match incoming events and route them to targets.
- Example targets: Lambda, Step Functions, SNS, SQS, EC2 actions.
- Example: On **RDS failover event**, trigger a Lambda function to send Slack notification.

---

## CloudWatch Logs Insights

- Interactive query capability for logs.
- SQL-like query language.
- Example:

```plaintext
  fields @timestamp, @message
  | filter @message like /ERROR/
  | sort @timestamp desc
  | limit 20
```

---

## CloudWatch Contributor Insights

- Analyze **who or what** is contributing the most to a metric.
- Example: Identify which IP is causing high API Gateway 5xx errors.

---

## CloudWatch Synthetics

- Canaries: scripted tests to simulate user actions.
- Monitor endpoints and APIs proactively.
- Detect issues before customers are impacted.

---

## CloudWatch Application Insights

- Automatically sets up monitoring for common application stacks (e.g., Java, .NET, databases).
- Detects and alerts on common problems.

---

## Monitoring Examples

### EC2 Instance Monitoring

- By default, CloudWatch provides metrics every **5 minutes**.
- With detailed monitoring enabled: **1-minute granularity**.
- Metrics: `CPUUtilization`, `NetworkIn`, `DiskReadOps`.

### RDS Monitoring

- Metrics: `CPUUtilization`, `FreeableMemory`, `DatabaseConnections`.
- Performance Insights integration for query-level analysis.

---

## Best Practices

- Use **tags** to organize metrics and logs per environment (dev, test, prod).
- Aggregate logs from multiple sources into CloudWatch Logs.
- Enable **detailed monitoring** for critical workloads.
- Set alarms with **multiple actions** (e.g., SNS + Auto Scaling).
- Use **dashboards** for real-time visualization.
- Optimize costs by setting **log retention policies**.

---

## Summary

- **CloudWatch Metrics** → monitor system and application performance.
- **CloudWatch Logs** → centralize log management.
- **Alarms** → automated responses to performance thresholds.
- **Dashboards** → visualize performance data.
- **Events/EventBridge** → automate workflows.
- **CloudWatch Agent** → extend monitoring to OS-level metrics and custom logs.

CloudWatch is the central monitoring service for AWS, enabling observability across infrastructure, applications, and
business KPIs.  
