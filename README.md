# Assignment 2.12 Exploring Function as a Service (FaaS)

## 1. What is the purpose of the execution role on the Lambda function?

The **execution role** is an IAM role that the Lambda function assumes during runtime. It grants the function permission to interact with other AWS services.

### Common Permissions:
- Read/write data from S3 buckets
- Publish messages to SNS topics
- Write logs to CloudWatch

Without this role, the function operates in a sandboxed environment and cannot access external AWS resources.

---

## 2. Resource-Based Policy Purpose

A **resource-based policy** is attached directly to the Lambda function and controls **who or what can invoke it**.

### 🧩 Example Use Cases:
- Allowing S3 to invoke the function when a new object is uploaded
- Enabling API Gateway to trigger the function via HTTP requests

This policy is essential for cross-service or cross-account invocation scenarios.

---

## 3. If the function is needed to upload a file into an S3 bucket, describe (i.e no need for the actual policies)

The Lambda function must have permissions to do write operations on the target S3 bucket. example: adding objects to the bucket.This can be done by updating the execution role. This ensures the function's code can securely interact with S3 during execution without access denied errors.

- The **execution role** must include permissions to perform write operations on the target bucket (e.g., `s3:PutObject`).
- This ensures secure and seamless interaction with S3 during function execution.

---

## What is the needed update on the execution role?

To support file uploads to S3, update the execution role to include:

```json
{
  "Effect": "Allow",
  "Action": "s3:PutObject",
  "Resource": "arn:aws:s3:::your-target-bucket-name/*"
}
```
## What is the new resource-based policy that needs to be added (if any)?
- if S3 is triggering the Lambda upon file upload, then S3 bucket needs a resource-based policy or event notification configuration that allows it to invoke the lambda function.
