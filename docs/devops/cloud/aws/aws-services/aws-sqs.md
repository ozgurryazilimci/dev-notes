# AWS Services - Simple Queue Service (SQS)

## Overview

**Amazon SQS (Simple Queue Service)** is a **fully managed message queuing service** that enables decoupling and scaling
of microservices, distributed systems, and serverless applications.

Key features:

- **Decoupling**: Break dependencies between producers and consumers.
- **Scalable**: Unlimited throughput, scales automatically.
- **Durable**: Messages stored redundantly across multiple Availability Zones.
- **Secure**: IAM access control, encryption, VPC endpoints.

---

## Core Concepts

- **Queue**: A temporary repository for messages.
- **Producer**: Component that sends messages to a queue.
- **Consumer**: Component that retrieves and processes messages.
- **Message**: Data sent by producers and consumed by consumers.
- **Visibility Timeout**: Time a message is hidden after being retrieved, preventing duplicate processing.
- **Dead-Letter Queue (DLQ)**: Stores messages that cannot be successfully processed after multiple retries.

---

## Queue Types

### 1. Standard Queue

- High throughput (unlimited transactions per second).
- At-least-once delivery (possible duplicates).
- Best-effort ordering (not guaranteed).

### 2. FIFO Queue

- Exactly-once processing (no duplicates).
- Preserves message order.
- Limited throughput (up to 3,000 messages/second with batching).

---

## Use Cases

- Decoupling microservices.
- Buffering requests between producers and consumers.
- Asynchronous job processing.
- Order processing with guaranteed delivery.
- Event-driven systems when combined with SNS or Lambda.

---

## Creating an SQS Queue (Console)

1. Open **Amazon SQS** in the AWS Console.
2. Choose **Create queue**.
3. Select queue type (Standard or FIFO).
4. Configure:
    - Name
    - Message retention period
    - Visibility timeout
    - Delivery delay
    - Encryption (KMS)
    - Dead-letter queue (optional)

---

## Sending and Receiving Messages

### CLI - Send a Message

    aws sqs send-message \
      --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/MyQueue \
      --message-body "Hello from SQS"

### CLI - Receive a Message

    aws sqs receive-message \
      --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/MyQueue \
      --max-number-of-messages 1 \
      --visibility-timeout 30 \
      --wait-time-seconds 10

### CLI - Delete a Message

    aws sqs delete-message \
      --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/MyQueue \
      --receipt-handle <receipt-handle>

---

## Dead-Letter Queues (DLQ)

- Messages failing processing after `maxReceiveCount` are moved to the DLQ.
- Helps with debugging and avoiding infinite retries.
- Best practice: Always configure DLQs for production queues.

---

## Message Lifecycle

1. Producer sends message to a queue.
2. Message stored redundantly across multiple AZs.
3. Consumer retrieves message (visibility timeout starts).
4. Consumer processes message.
5. If successful → consumer deletes message.
6. If not deleted → message reappears in queue after timeout.

---

## Security

- **Access Control**: Manage with IAM policies.
- **Encryption**:
    - At rest: AWS KMS.
    - In transit: TLS.
- **VPC Endpoints**: Access queues securely within a VPC.
- **Policies**: Restrict who can send/receive messages.

---

## Monitoring and Logging

- **CloudWatch Metrics**:
    - NumberOfMessagesSent
    - NumberOfMessagesReceived
    - ApproximateNumberOfMessagesVisible
    - ApproximateNumberOfMessagesDelayed
    - ApproximateNumberOfMessagesNotVisible
- **CloudTrail**: Logs SQS API calls.
- **CloudWatch Alarms**: Alert on backlog growth or processing failures.

---

## Best Practices

- Use **FIFO queues** for ordered, exactly-once delivery.
- Use **long polling** (`--wait-time-seconds`) to reduce costs and empty responses.
- Configure **Dead-Letter Queues** for error handling.
- Apply **visibility timeout** long enough for processing but short enough to retry failures.
- Batch messages to improve throughput and reduce API costs.
- Use **server-side encryption** for sensitive data.

---

## Example Workflow

1. Application (Producer) sends an "Order Created" message to `OrdersQueue`.
2. Worker service (Consumer) receives and processes the order.
3. If processing fails multiple times, message moves to a DLQ (`OrdersDLQ`).
4. Monitoring with CloudWatch ensures backlog is under control.
5. Alerts notify developers if DLQ grows unexpectedly.

---

## Summary

- **Amazon SQS** is a reliable, fully managed message queuing service.
- Provides **Standard (high throughput)** and **FIFO (ordered, exactly-once)** queues.
- Ensures durability, scalability, and fault tolerance.
- Works seamlessly with **SNS, Lambda, EC2, and ECS** in event-driven architectures.  
