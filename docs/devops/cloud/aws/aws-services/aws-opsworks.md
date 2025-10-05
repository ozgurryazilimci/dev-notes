# AWS OpsWorks

AWS OpsWorks is a configuration management service that provides managed instances of **Chef** and **Puppet**. It helps
you automate server configuration, deployment, and management across Amazon EC2 instances or on-premises servers.

This guide provides an overview of AWS OpsWorks, including how to set it up using the **AWS Console (UI)** and how to
interact with it programmatically using the **AWS CLI and SDKs**.

---

## Key Concepts

- **Stacks**: A container for resources such as EC2 instances, layers, and applications.
- **Layers**: Define how resources are set up and managed (e.g., load balancer layer, database layer, application server
  layer).
- **Instances**: EC2 instances managed by OpsWorks, associated with specific layers.
- **Apps**: Applications deployed on layers (e.g., web applications).
- **Chef/Puppet Integration**: Enables automation of configuration management tasks.

---

## Using AWS Console (UI)

### Create an OpsWorks Stack

1. Open the **AWS Management Console**.
2. Navigate to **OpsWorks**.
3. Click **Add Stack**.
4. Choose **Chef** or **Puppet** as your configuration management engine.
5. Provide:
    - **Stack name**
    - **Region**
    - **Default operating system** (e.g., Amazon Linux, Ubuntu)
    - **Default SSH key pair**
6. Create the stack.

### Add a Layer

1. Within the stack, click **Add a layer**.
2. Choose a layer type (e.g., Node.js App Server, PHP App Server, MySQL).
3. Configure instance type, auto-healing, load balancer, etc.
4. Save the layer.

### Add an Instance

1. Go to the **Instances** tab in your stack.
2. Click **Add an instance**.
3. Associate the instance with a **layer**.
4. Choose instance size, availability zone, and settings.
5. Start the instance.

### Deploy an Application

1. In your stack, go to the **Apps** tab.
2. Click **Add an app**.
3. Provide:
    - **App name**
    - **Repository type** (Git, S3, HTTP archive)
    - **Repository URL**
4. Deploy the app to a layer.

---

## 3. Developer (SDK/CLI) Usage

### Create a Stack (AWS CLI)

```bash
aws opsworks create-stack       --name "MyStack"       --region us-east-1       --service-role-arn arn:aws:iam::123456789012:role/aws-opsworks-service-role       --default-instance-profile-arn arn:aws:iam::123456789012:instance-profile/aws-opsworks-ec2-role       --default-os "Amazon Linux 2"       --default-root-device-type ebs
```

### Add a Layer

```bash
aws opsworks create-layer       --stack-id <stack-id>       --type custom       --name "AppLayer"       --shortname "app-layer"
```

### Register an Instance

```bash
aws opsworks register-instance       --stack-id <stack-id>       --hostname MyInstance       --public-ip 203.0.113.25
```

### Deploy an App

```bash
aws opsworks create-app       --stack-id <stack-id>       --name "MyWebApp"       --type other       --app-source "Type=git,Url=https://github.com/example/myapp.git"
```

---

## Best Practices

- **Use Service Roles**: Ensure OpsWorks has the correct IAM service role and EC2 instance profile.
- **Layer Separation**: Organize applications, databases, and load balancers into separate layers for flexibility.
- **Monitoring**: Enable **CloudWatch metrics** and OpsWorks built-in monitoring.
- **Automation**: Use Chef/Puppet recipes for automatic configuration and updates.
- **Security**: Control SSH access with IAM users and keys.

---

## Common Use Cases

- **Web Application Deployment**
    - Deploy multi-tier web applications using layers for app servers, databases, and load balancers.
- **Configuration Management**
    - Automate setup of servers using Chef or Puppet recipes.
- **Hybrid Environments**
    - Manage both on-premises and AWS infrastructure from a single service.
- **CI/CD Integration**
    - Deploy code automatically from repositories like GitHub or AWS CodeCommit.

---

## Summary

AWS OpsWorks simplifies **infrastructure management** using Chef and Puppet. Developers can use:

- **AWS Console (UI)** for visual management of stacks, layers, and apps.
- **AWS CLI & SDKs** for automation and integration into CI/CD pipelines.

OpsWorks is ideal for teams looking for **configuration automation**, **consistent deployments**, and **hybrid
infrastructure management** across AWS and on-premises environments.
