# AWS Services - Simple Workflow Service (SWF)

## Overview

**Amazon SWF (Simple Workflow Service)** is a fully managed service for **building, running, and scaling
background jobs** that have parallel or sequential steps.  
It coordinates the execution of tasks across distributed components, ensuring state management, retries, and tracking.

Key features:

- Reliable task coordination
- Human and automated tasks
- State tracking and history
- Retries and error handling
- Integration with other AWS services

---

## Core Concepts

- **Workflow**: A set of steps (activities) executed in a defined order.
- **Workflow Execution**: A running instance of a workflow definition.
- **Workflow Starter**: An application that starts a workflow execution.
- **Decider**: A program that controls the flow of execution (decides which activity/task to run next).
- **Activity Worker**: A component that performs a task (could be human or automated).
- **Task**: A single unit of work, either:
    - **Activity Task**: Work done by activity workers.
    - **Decision Task**: Work done by deciders.
- **Domain**: A namespace for workflows.

---

## Use Cases

- Order processing pipelines
- Video or image processing workflows
- Data ETL pipelines
- Business processes requiring human + system steps
- Long-running background jobs

---

## Workflow Example

**Scenario**: Order Processing

1. Customer places an order → **Workflow Starter** starts a workflow.
2. Workflow executes activities:
    - Verify Payment (Activity Worker 1)
    - Reserve Inventory (Activity Worker 2)
    - Ship Order (Activity Worker 3)
3. **Decider** coordinates task execution and handles retries on failures.
4. Workflow completes when all tasks succeed.

---

## Creating a Domain

    aws swf register-domain \
      --name OrderProcessingDomain \
      --description "Domain for order processing workflows" \
      --workflow-execution-retention-period-in-days 10

---

## Registering a Workflow Type

    aws swf register-workflow-type \
      --domain OrderProcessingDomain \
      --name OrderWorkflow \
      --version v1 \
      --default-task-start-to-close-timeout 300

---

## Registering an Activity Type

    aws swf register-activity-type \
      --domain OrderProcessingDomain \
      --name VerifyPayment \
      --version v1 \
      --default-task-start-to-close-timeout 300

---

## Starting a Workflow Execution

    aws swf start-workflow-execution \
      --domain OrderProcessingDomain \
      --workflow-type name=OrderWorkflow,version=v1 \
      --workflow-id order-12345 \
      --task-list name=MainTaskList

---

## Polling for Tasks

- **Activity Worker** polls for activity tasks:

```bash
aws swf poll-for-activity-task \
  --domain OrderProcessingDomain \
  --task-list name=MainTaskList \
  --identity Worker1
```

- **Decider** polls for decision tasks:

```bash
aws swf poll-for-decision-task \
  --domain OrderProcessingDomain \
  --task-list name=MainTaskList \
  --identity Decider1
```

---

## Completing Tasks

- **Complete Activity Task**:

```bash
aws swf respond-activity-task-completed \
  --task-token <task-token> \
  --result "Payment verified"
```

- **Complete Decision Task**:

```bash
aws swf respond-decision-task-completed \
  --task-token <task-token> \
  --decisions <decisions-json>
```

---

## Monitoring and Logging

- **Amazon SWF Console**:
    - Workflow execution history
    - Running and completed workflows
    - Activity/task failures
- **CloudWatch**: Monitor metrics such as task counts and latencies.
- **CloudTrail**: Logs API activity.

---

## Comparison with Step Functions

- **SWF**:
    - More control over task execution
    - Supports human tasks
    - Low-level APIs
    - Requires more coding effort (Deciders, Workers)
- **Step Functions**:
    - Higher-level service with state machine design
    - Fully managed orchestration
    - Visual workflow builder
    - Easier integration with AWS services

**Recommendation**: Use **Step Functions** for most new projects. Use **SWF** if you require **human task coordination**
or have legacy workflows built on it.

---

## Best Practices

- Use **task lists** to separate workflows and workers.
- Implement retries and error handling in deciders.
- Secure domains with IAM policies.
- Use **CloudWatch alarms** to track failures or delays.
- Consider **Amazon Step Functions** if you need easier orchestration.

---

## Summary

- **Amazon SWF** coordinates complex workflows with both automated and human tasks.
- Provides reliable state tracking, retries, and audit trails.
- Requires custom **deciders** and **workers** to run workflows.
- Still useful for long-running workflows, but **Step Functions** is often recommended for new applications.  
