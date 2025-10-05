# AWS Container Services

AWS offers multiple services to run and manage containerized applications at scale. The main services include:

- **Amazon ECS (Elastic Container Service)**: A fully managed container orchestration service.
- **Amazon EKS (Elastic Kubernetes Service)**: A managed Kubernetes service.
- **Amazon ECR (Elastic Container Registry)**: A fully managed Docker container registry.
- **AWS Fargate**: A serverless compute engine for containers, works with ECS and EKS.

This guide explains how these services fit together, how to use them with the **AWS Console (UI)**, and programmatically
with the **AWS CLI and SDKs**.

---

## Amazon Elastic Container Service (ECS)

### Overview

- Fully managed container orchestration service.
- Supports **EC2 launch type** (you manage the EC2 instances) or **Fargate launch type** (serverless).
- Uses **tasks** (definitions of containers) and **services** (long-running containers).

### Console (UI) Setup

1. Open **ECS Console**.
2. Click **Create cluster**.
    - Choose **EC2** or **Fargate**.
3. Create a **Task Definition**:
    - Specify container image (from ECR, Docker Hub, or custom).
    - Define CPU/memory.
    - Add environment variables and networking.
4. Create a **Service**:
    - Choose cluster, task definition, number of tasks.
    - Configure load balancer (optional).
5. Deploy and monitor your containers.

### CLI Example: Run ECS Task

```bash
aws ecs run-task \
  --cluster myCluster \
  --launch-type FARGATE \
  --network-configuration "awsvpcConfiguration={subnets=[subnet-12345],securityGroups=[sg-12345],assignPublicIp=ENABLED}" \
  --task-definition myTaskDef
```

---

## Amazon Elastic Kubernetes Service (EKS)

### Overview

- Managed **Kubernetes control plane**.
- Integrates with AWS services like IAM, VPC, CloudWatch, and Fargate.
- Developers use standard **kubectl** commands.

### Console (UI) Setup

1. Open **EKS Console** → **Add Cluster**.
2. Configure:
    - **Cluster name**
    - **Kubernetes version**
    - **IAM role** for cluster
3. Configure node groups:
    - EC2 managed nodes or Fargate profiles.
4. Launch the cluster and connect via `kubectl`.

### CLI Example: Create Cluster

```bash
aws eks create-cluster \
  --name myCluster \
  --role-arn arn:aws:iam::123456789012:role/EKSClusterRole \
  --resources-vpc-config subnetIds=subnet-abc123,securityGroupIds=sg-abc123
```

### Kubectl Example

```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=LoadBalancer
```

---

## Amazon Elastic Container Registry (ECR)

### Overview

- Managed **Docker container registry**.
- Secure, private, and integrated with IAM.
- Supports vulnerability scanning and lifecycle policies.

### Console (UI) Setup

1. Open **ECR Console**.
2. Click **Create repository**.
3. Configure:
    - Repository name
    - Tag immutability
    - Scan on push
4. Push images using Docker CLI.

### CLI Example: Push Image

```bash
aws ecr create-repository --repository-name my-app

aws ecr get-login-password --region us-east-1 \
  | docker login --username AWS --password-stdin <account-id>.dkr.ecr.us-east-1.amazonaws.com

docker build -t my-app .
docker tag my-app:latest <account-id>.dkr.ecr.us-east-1.amazonaws.com/my-app:latest
docker push <account-id>.dkr.ecr.us-east-1.amazonaws.com/my-app:latest
```

---

## AWS Fargate

### Overview

- **Serverless compute engine** for containers.
- Runs containers without provisioning or managing servers.
- Works with both **ECS** and **EKS**.

### Console (UI) Setup with ECS

1. Open **ECS Console**.
2. Create a **Task Definition** with **Fargate launch type**.
3. Create a **Service** and select **Fargate**.
4. Deploy containers with AWS-managed compute.

### CLI Example: Run Container with Fargate

```bash
aws ecs run-task \
  --cluster myCluster \
  --launch-type FARGATE \
  --task-definition myTaskDef \
  --network-configuration "awsvpcConfiguration={subnets=[subnet-xyz],securityGroups=[sg-xyz],assignPublicIp=ENABLED}"
```

---

## Best Practices

- **ECR Security**: Enable image scanning and enforce tag immutability.
- **Networking**: Use VPC, private subnets, and security groups for secure deployments.
- **Monitoring**: Use CloudWatch, Container Insights, and X-Ray for observability.
- **Autoscaling**: Use Application Auto Scaling for ECS services or Kubernetes Cluster Autoscaler for EKS.
- **CI/CD**: Integrate with AWS CodePipeline, GitHub Actions, or Jenkins to automate builds and deployments.

---

## Common Use Cases

- **Microservices Architecture**: Run multiple isolated services in ECS/EKS.
- **Hybrid Deployments**: Run workloads across on-premises and AWS with EKS Anywhere.
- **Batch Processing**: Use ECS/EKS with Fargate for event-driven workloads.
- **CI/CD Pipelines**: Build and push Docker images to ECR, deploy automatically to ECS/EKS.

---

## Summary

AWS Container Services provide flexibility for container orchestration, registry, and compute:

- **ECS**: AWS-native container orchestration.
- **EKS**: Kubernetes orchestration.
- **ECR**: Secure image storage and management.
- **Fargate**: Serverless container compute.

By combining these services, developers can build **scalable, secure, and automated containerized applications** in the
cloud.
