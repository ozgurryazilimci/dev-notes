# AWS Services - CloudFormation

## Overview

**AWS CloudFormation** is a service that allows you to model and provision AWS resources as code.  
It provides a **declarative language** (JSON or YAML) to define your infrastructure in **templates**, making it easier
to automate, reproduce, and manage resources.

Key benefits:

- **Infrastructure as Code (IaC)**: Version-controlled, repeatable infrastructure.
- **Automation**: Provision resources consistently without manual configuration.
- **Scalability**: Reuse templates across environments (dev, staging, prod).
- **Integration**: Works with CI/CD pipelines (e.g., CodePipeline, GitHub Actions).

---

## Core Concepts

- **Template**: JSON or YAML file describing resources (e.g., EC2, S3, RDS).
- **Stack**: A collection of resources created from a template.
- **Change Set**: A preview of changes that will be applied before executing updates.
- **Parameters**: User-provided input values to customize stacks.
- **Outputs**: Values returned after stack creation (e.g., endpoint URLs).
- **Mappings**: Key-value maps for conditional configuration (e.g., region-to-AMI mapping).
- **Conditions**: Logic to control resource creation (e.g., create only in prod).
- **Resources**: The actual AWS services (mandatory section in every template).

---

## How It Works

1. Write a **template** (YAML/JSON).
2. Upload it to CloudFormation (via Console, CLI, or SDK).
3. CloudFormation creates a **stack** containing all defined resources.
4. When updating, you can use a **change set** to preview differences.
5. Delete a stack to remove all associated resources.

---

## Example CloudFormation Template (YAML)

    AWSTemplateFormatVersion: '2010-09-09'
    Description: Simple S3 Bucket Example
    Resources:
      MyS3Bucket:
        Type: AWS::S3::Bucket
        Properties:
          BucketName: my-example-bucket

---

## Using Parameters and Outputs

    Parameters:
      InstanceType:
        Type: String
        Default: t2.micro
        AllowedValues:
          - t2.micro
          - t3.micro
          - t3.small
        Description: EC2 instance type

    Resources:
      MyEC2Instance:
        Type: AWS::EC2::Instance
        Properties:
          InstanceType: !Ref InstanceType
          ImageId: ami-0abcdef1234567890

    Outputs:
      InstanceId:
        Description: The Instance ID
        Value: !Ref MyEC2Instance

---

## Stack Operations

- **Create Stack**: Launch resources from a template.
- **Update Stack**: Modify resources; CloudFormation will determine what to replace or update.
- **Delete Stack**: Removes all resources in the stack.
- **Rollback**: If stack creation fails, CloudFormation rolls back all changes automatically.

---

## CloudFormation Designer

- A **visual tool** in the AWS Console.
- Drag-and-drop interface for creating/editing templates.
- Helps visualize relationships between resources.

---

## Change Sets

- Allows you to preview how changes will affect existing resources.
- Useful for minimizing downtime and avoiding accidental deletion.

```bash
aws cloudformation create-change-set \
--stack-name MyStack \
--change-set-name MyChangeSet \
--template-body file://template.yaml
```

---

## Nested Stacks

- Break large templates into **smaller, reusable stacks**.
- Example: One template for networking, another for compute, linked together.

---

## StackSets

- Deploy a single CloudFormation template across **multiple accounts and regions**.
- Great for enterprise-wide standardization.

---

## Integration with Other AWS Services

- **CloudWatch Events / EventBridge**: Trigger stack operations automatically.
- **CodePipeline**: Automate deployment in CI/CD workflows.
- **Service Catalog**: Publish standardized templates for teams.

---

## Best Practices

- Use **YAML** for readability (comments supported).
- Store templates in **source control (GitHub, CodeCommit)**.
- Apply **naming conventions** and **tagging** for resources.
- Test with **dev/staging** before deploying to production.
- Leverage **parameters** for flexibility and **mappings** for region-specific configs.
- Enable **stack termination protection** for critical stacks.
- Keep templates modular with **nested stacks**.

---

## Common CLI Commands

Create a stack:

    aws cloudformation create-stack \
      --stack-name my-stack \
      --template-body file://template.yaml \
      --parameters ParameterKey=InstanceType,ParameterValue=t2.micro

Update a stack:

    aws cloudformation update-stack \
      --stack-name my-stack \
      --template-body file://template.yaml

Delete a stack:

    aws cloudformation delete-stack \
      --stack-name my-stack

---

## Summary

- **CloudFormation** is the foundation of IaC in AWS.
- Templates define resources in a declarative way (JSON/YAML).
- Provides automation, scalability, and repeatability.
- Supports advanced features like **change sets, nested stacks, and StackSets**.
- Ideal for consistent multi-environment deployments and enterprise governance.  
