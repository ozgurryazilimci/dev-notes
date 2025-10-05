# AWS Lambda & API Gateway

AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers. Amazon API
Gateway is a fully managed service that enables developers to create, publish, maintain, and secure APIs at scale.
Together, they allow you to build scalable serverless applications with minimal infrastructure management.

This guide provides an overview of **AWS Lambda** and **API Gateway**, including how to set them up using the **AWS
Console (UI)** and how to use them programmatically with the **AWS CLI and SDKs**. It also includes an example of
invoking Lambda from API Gateway.

---

## AWS Lambda Overview

- **Event-driven** compute service that executes code in response to events.
- Supports multiple runtimes (Python, Node.js, Java, Go, .NET, Ruby).
- Automatically scales with demand.
- Billed per request and execution duration.

### Key Components

- **Function**: The unit of deployment (your code).
- **Trigger**: Event source that invokes the Lambda (e.g., API Gateway, S3, DynamoDB, CloudWatch).
- **Execution Role**: IAM role Lambda uses to access other AWS services.

---

## API Gateway Overview

- Fully managed service for creating and deploying **REST, HTTP, and WebSocket APIs**.
- Provides **authentication, authorization, rate limiting, caching, monitoring, and logging**.
- Can trigger **AWS Lambda functions** directly as backends.

---

## Using AWS Console (UI)

### Create a Lambda Function

1. Open the **AWS Management Console**.
2. Navigate to **Lambda** → **Functions** → **Create function**.
3. Choose **Author from scratch**.
4. Enter function name (e.g., `HelloWorldLambda`).
5. Choose runtime (e.g., Python 3.10).
6. Choose or create an **execution role**.
7. Click **Create function**.
8. Add handler code (example Python):

        def lambda_handler(event, context):
            return {
                'statusCode': 200,
                'body': 'Hello from Lambda!'
            }

9. Click **Deploy**.

### Create an API Gateway to Trigger Lambda

1. In the AWS Console, navigate to **API Gateway**.
2. Click **Create API** → choose **HTTP API**.
3. Provide an API name (e.g., `HelloWorldAPI`).
4. Under **Integrations**, choose **Lambda** and select your function.
5. Configure routes (e.g., `GET /hello`).
6. Deploy the API.
7. Copy the endpoint URL and test with a browser or `curl`.

---

## Developer (SDK/CLI) Usage

### Invoke Lambda with AWS CLI

```bash
aws lambda invoke \
  --function-name HelloWorldLambda \
  --payload '{"key":"value"}' \
  response.json
```

### Deploy API Gateway with AWS CLI

```bash
aws apigatewayv2 create-api \
  --name "HelloWorldAPI" \
  --protocol-type HTTP \
  --target arn:aws:lambda:us-east-1:123456789012:function:HelloWorldLambda
```

### Python (boto3) Example: Invoke Lambda

```python
import boto3
import json

client = boto3.client('lambda')

response = client.invoke(
    FunctionName='HelloWorldLambda',
    Payload=json.dumps({"message": "Hello"})
)

print(response['Payload'].read().decode())
```

---

## Example: API Gateway Triggering Lambda

### Step 1: Lambda Function (Python)

```python
def lambda_handler(event, context):
    name = event.get("queryStringParameters", {}).get("name", "World")
    return {
        'statusCode': 200,
        'body': f"Hello, {name}!"
    }
```

### Step 2: API Gateway Route

- Create route: `GET /greet`
- Integration: Link to the Lambda function.

### Step 3: Test the Endpoint

```bash
curl "https://<api-id>.execute-api.us-east-1.amazonaws.com/greet?name=John"
```

**Response:**

```json
{
  "statusCode": 200,
  "body": "Hello, John!"
}
```

---

## Best Practices

- **IAM Roles**: Use least-privilege IAM roles for Lambda and API Gateway execution.
- **Error Handling**: Implement structured error responses for Lambda functions.
- **Logging**: Use **CloudWatch Logs** for monitoring and debugging.
- **Security**: Protect APIs with **IAM auth, Cognito, or Lambda authorizers**.
- **Performance**: Keep functions lightweight and use **environment variables** for configuration.

---

## Common Use Cases

- **Serverless REST APIs** with Lambda backends.
- **Webhook processing** (e.g., from third-party services).
- **IoT data ingestion** via HTTP endpoints.
- **Microservices architecture** using multiple Lambda functions behind API Gateway.

---

## Summary

AWS Lambda and API Gateway form the backbone of **serverless applications**. Developers can use:

- **AWS Console (UI)** for quick setup and testing.
- **AWS CLI and SDKs** for automation and integration.

With API Gateway invoking Lambda functions, you can build **scalable, secure, and cost-effective serverless APIs** with
minimal operational overhead.
