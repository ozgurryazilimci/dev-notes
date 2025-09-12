# AWS Services - Simple Email Service (SES)

## Overview

**Amazon SES (Simple Email Service)** is a cost-effective, scalable, and flexible email-sending service that allows
developers to send transactional, marketing, or bulk emails.  
It provides high deliverability using AWS’s trusted infrastructure and supports both **SMTP** and **API-based** email
sending.

Key features:

- Transactional and marketing email support.
- High deliverability with reputation management.
- Domain and email identity verification.
- DKIM, SPF, and DMARC support.
- Bounces and complaints handling.
- Integration with AWS services (Lambda, S3, SNS, CloudWatch).

---

## Core Concepts

- **Identity**: An email address or domain you use to send emails. Identities must be verified before sending.
- **Sandbox Mode**: By default, SES starts in sandbox mode:
    - You can only send to verified email addresses.
    - There is a limit on sending rate and daily quota.
- **Production Mode**: Request AWS to move SES out of sandbox for real-world usage.
- **Domains & DKIM**: Use DKIM/DMARC/SPF for email authentication and better deliverability.

---

## Setup Steps

### 1. Verify an Email Address

Go to **SES Console → Identities → Verify Email Address**, or use CLI:

    aws ses verify-email-identity --email-address user@example.com

### 2. Verify a Domain

Add DNS records (TXT for SPF, CNAME for DKIM) provided by SES in your DNS service (e.g., Route53).

### 3. Request Production Access

Submit a request in the **SES Console → Sending Limits → Request production access**.

---

## Sending Emails

### Using AWS CLI

Send a basic email:

    aws ses send-email \
      --from "sender@example.com" \
      --destination "ToAddresses=recipient@example.com" \
      --message "Subject={Data=Hello from SES},Body={Text={Data=This is a test email}}"

Send raw email with attachments:

    aws ses send-raw-email --raw-message file://message.txt

### Using SMTP

1. Create SMTP credentials in the **SES Console**.
2. Use any SMTP client with the credentials and the provided endpoint (e.g., `email-smtp.us-east-1.amazonaws.com`).

Example with `sendmail`:

    sendmail -S email-smtp.us-east-1.amazonaws.com:587 \
    -au SMTP_USERNAME -ap SMTP_PASSWORD recipient@example.com

---

## Monitoring and Feedback

- **Bounces and Complaints**: Configure an **SNS topic** to receive feedback notifications.
- **CloudWatch Metrics**: Monitor sending statistics (send rate, bounces, complaints).
- **Reputation Dashboard**: View reputation metrics and maintain good sending practices.

---

## Best Practices

- Warm up new email domains and IPs by gradually increasing send volume.
- Always authenticate with **SPF, DKIM, DMARC**.
- Maintain low bounce and complaint rates (< 0.1%).
- Use suppression list to avoid sending to invalid addresses.
- Segment marketing vs transactional emails with configuration sets.

---

## Integration Examples

### Store Emails in S3

You can configure incoming emails (via SES Receipt Rules) to be stored in an **S3 bucket**.

### Trigger Lambda Functions

SES can trigger a **Lambda function** for processing inbound emails (e.g., parsing invoices).

### Notifications via SNS

Set up SNS topics for bounces, complaints, and delivery notifications.

---

## Useful Commands

Check verified identities:

    aws ses list-identities

Check sending quota:

    aws ses get-send-quota

Check sending statistics:

    aws ses get-send-statistics
