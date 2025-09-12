# AWS Services - Simple Notification Service (SNS)

## Overview

**Amazon SNS (Simple Notification Service)** is a **fully managed pub/sub messaging service** that enables applications,
microservices, and distributed systems to communicate asynchronously.

Key features:

- **Publish/Subscribe model**: One-to-many communication.
- **Multiple protocols**: HTTP/S, Email, SMS, Lambda, SQS, and Mobile Push.
- **Scalable**: Millions of messages per second.
- **Secure**: IAM policies, encryption, and message filtering.

---

## Core Concepts

- **Topic**: A communication channel for sending messages. Publishers send to a topic; subscribers receive from it.
- **Publisher**: Any component that sends messages to a topic.
- **Subscriber**: An endpoint (e.g., Lambda, SQS, Email, SMS) that receives messages from a topic.
- **Subscription**: The link between a topic and an endpoint.
- **Message**: The actual payload published to a topic.
- **Message Filtering**: Define rules (using message attributes) to control which subscribers receive which messages.

---

## Use Cases

- **Application-to-Application (A2A)**:
    - Microservices communication.
    - Triggering workflows across systems.
    - Fan-out messaging (one message → multiple services).
- **Application-to-Person (A2P)**:
    - Notifications via SMS, Email, or Push.
    - System alerts and monitoring.
    - Marketing or customer engagement messages.

---

## Creating a Topic (Console)

1. Open **Amazon SNS** in AWS Console.
2. Choose **Topics > Create topic**.
3. Select type:
    - **Standard**: High throughput, best-effort ordering, possible duplicates.
    - **FIFO**: First-In-First-Out, exactly-once processing.
4. Configure:
    - Name
    - Display name (for SMS)
    - Encryption (optional)
5. Create the topic.

---

## Subscribing to a Topic

1. Select your topic.
2. Click **Create subscription**.
3. Choose a protocol:
    - HTTP/S
    - Email/Email-JSON
    - SMS
    - AWS Lambda
    - Amazon SQS
4. Enter endpoint (e.g., email address, SQS ARN).
5. Confirm subscription (for Email/SMS protocols, confirmation is required).

---

## Publishing Messages

- Via Console: Select a topic → **Publish message** → Enter subject & message body.
- Via CLI:

```bash
aws sns publish \
  --topic-arn arn:aws:sns:us-east-1:123456789012:MyTopic \
  --message "Hello from SNS"
```

- With message attributes (for filtering):

```bash
aws sns publish \
  --topic-arn arn:aws:sns:us-east-1:123456789012:MyTopic \
  --message "Order received" \
  --message-attributes '{"orderType":{"DataType":"String","StringValue":"express"}}'
```

---

## Message Filtering

- Subscribers can filter by attributes.
- Example: Only receive messages where `orderType = express`.

```json
{
  "orderType": [
    "express"
  ]
}
```

---

## Dead-Letter Queues (DLQs)

- SNS can send undeliverable messages to an **SQS DLQ**.
- Useful for debugging delivery failures.

---

## Security

- **Access Control**: Use IAM policies for publish/subscribe permissions.
- **Encryption**:
    - At rest: KMS-managed keys.
    - In transit: HTTPS.
- **Delivery Policies**: Control retry behavior and backoff strategies.

---

## Monitoring and Logging

- **CloudWatch Metrics**:
    - NumberOfMessagesPublished
    - NumberOfNotificationsDelivered
    - NumberOfNotificationsFailed
- **CloudWatch Alarms**: Trigger alerts on delivery failures or throughput.
- **CloudTrail**: Logs API calls (e.g., CreateTopic, Publish).

---

## CLI Examples

Create a topic:

    aws sns create-topic --name MyTopic

Subscribe to the topic (Email):

    aws sns subscribe \
      --topic-arn arn:aws:sns:us-east-1:123456789012:MyTopic \
      --protocol email \
      --notification-endpoint myemail@example.com

List subscriptions:

    aws sns list-subscriptions-by-topic \
      --topic-arn arn:aws:sns:us-east-1:123456789012:MyTopic

Unsubscribe:

    aws sns unsubscribe --subscription-arn <subscription-arn>

Delete a topic:

    aws sns delete-topic --topic-arn arn:aws:sns:us-east-1:123456789012:MyTopic

---

## Best Practices

- Use **FIFO topics** when message order and deduplication are critical.
- Implement **message filtering** to reduce unnecessary processing.
- Integrate with **SQS** for durable storage and replay capabilities.
- Monitor delivery with **CloudWatch metrics and DLQs**.
- Secure topics with **IAM policies** and **KMS encryption**.
- For high fan-out, use **SNS → SQS** pattern (one topic → many queues).

---

## Summary

- **Amazon SNS** provides reliable, scalable pub/sub messaging.
- Supports multiple delivery protocols: HTTP/S, Email, SMS, Lambda, SQS.
- Features include message filtering, DLQs, and FIFO topics.
- Works well with **CloudWatch, CloudTrail, SQS, and Lambda** for end-to-end event-driven architectures.  
