## How do you ensure the security and compliance of your CI/CD pipelines in aws

I secure CI/CD pipelines by integrating DevSecOps practices:

**1: Use IAM roles and least privilege access** Assign the EC2 instance an IAM Role with minimal ECR + EKS deploy permissions.

**2: Store secrets in AWS Secrets Manager** store sensitive credentials like DB passwords, API keys, or tokens in AWS Secrets Manager, which securely encrypts them with KMS, rotates them automatically, and allows controlled access via IAM policies instead of hardcoding in code or pipelines

**3 Scan code and images for vulnerabilities** Continuously rescan images in your registry (not just at build time).

**4 Enforce policies with tools like Sentinel** Using Sentinel, you can enforce compliance/security as part of your CI/CD pipelines by defining policies as code.  

**5 Implement approval gates and audit logging** Implement approval gates by requiring manual checks before production deploys, and enable audit logging (via CloudWatch or CI/CD logs) to track who approved.

**6 Monitor pipeline activity with Cloud watch** Monitor pipeline activity using centralized logging and monitoring tools like AWS CloudWatch, CI/CD audit logs to track builds, deployments, failures, and security events in real time.

