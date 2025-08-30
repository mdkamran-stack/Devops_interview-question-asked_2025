## Tell me abt you CI/CD process that you have implemented.

In our organization we have Github as source code repository if any developers commit a code in source code repository jenkins pipeline will automatically triggered using Webhook as a first stage it will start pulling the code from source code repository once the code is pulled and checkedout next step building this code we use Maven as a part of this process once maven build the application, then we will check the code quality by using staticu code analysis will check application is secure or not after that we use toll App scan this is used for SAT & DAST after that we promote this Application to dev Env using ARGOCD & k8s Argco-cd is looking for k8s manifest in git repository whenever there is change once the new image is updated Argocd will look for new tag in the image using HELM chart it would deploy to new version of an application into target k8s cluster.

## How do you ensure the security and compliance of your CI/CD pipelines in aws

I secure CI/CD pipelines by integrating DevSecOps practices:

**1: Use IAM roles and least privilege access** Assign the EC2 instance an IAM Role with minimal ECR + EKS deploy permissions.

**2: Store secrets in AWS Secrets Manager** store sensitive credentials like DB passwords, API keys, or tokens in AWS Secrets Manager, which securely encrypts them with KMS, rotates them automatically, and allows controlled access via IAM policies instead of hardcoding in code or pipelines

**3 Scan code and images for vulnerabilities** Continuously rescan images in your registry (not just at build time).

**4 Enforce policies with tools like Sentinel** Using Sentinel, you can enforce compliance/security as part of your CI/CD pipelines by defining policies as code.  

**5 Implement approval gates and audit logging** Implement approval gates by requiring manual checks before production deploys, and enable audit logging (via CloudWatch or CI/CD logs) to track who approved.

**6 Monitor pipeline activity with Cloud watch** Monitor pipeline activity using centralized logging and monitoring tools like AWS CloudWatch, CI/CD audit logs to track builds, deployments, failures, and security events in real time.


## How to Manage Secrets Across Environments in Kubernetes (AWS) for interview purpose in short

In Kubernetes on AWS, manage secrets per environment using AWS Secrets Manager, inject them into pods via Kubernetes Secrets or IAM Roles for Service Accounts (IRSA), and avoid hardcoding credentials, ensuring least privilege access and automated rotation

