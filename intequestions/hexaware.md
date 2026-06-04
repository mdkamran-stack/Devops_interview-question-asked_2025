## You are tasked with onboarding a new business unit that requires three separate environments (Dev, Test, Prod).
How would you use AWS Control Tower and Account Factory to ensure these accounts automatically adhere to corporate security guardrails from day one?  

I would use AWS Control Tower to establish a governed multi-account environment and create a dedicated Organizational Unit for the new business unit
. Using Account Factory, I would provision separate Dev, Test, and Prod accounts with standardized configurations. Control Tower automatically 
applies security baselines such as CloudTrail, AWS Config, centralized logging, and IAM Identity Center integration. I would then apply guardrails
and Service Control Policies at the OU level to enforce corporate security requirements such as encryption, approved AWS regions, logging, and 
compliance controls. This ensures that every account is secure, compliant, and follows organizational standards from day one without requiring
manual setup.

##  A developer accidentally launches an unencrypted S3 bucket in a member account. Describe how you would use AWS 
Config and specialized Remediation Actions to automatically detect and fix this non-compliant resource without manual intervention.  

I would use AWS Config with the managed rule s3-bucket-server-side-encryption-enabled to continuously monitor S3 buckets across the organization. 
If a developer creates an unencrypted bucket, AWS Config automatically marks it as non-compliant. I would then configure an automatic remediation 
action using AWS Systems Manager Automation. The runbook would enable server-side encryption, either using SSE-S3 or SSE-KMS based on 
organizational standards. Once remediation is completed, AWS Config re-evaluates the bucket and marks it compliant. This provides a fully 
automated detect-and-remediate solution without requiring manual intervention and helps maintain security compliance across all AWS accounts.  

## Your organization needs to ensure that no resources are ever created outside of the us-east-1 and eu-west-1
regions. How would you implement this restriction across the entire AWS Organization using Control Tower or Service Control Policies (SCPs)?  

To enforce region restrictions across the entire AWS Organization, I would use a Service Control Policy (SCP) attached at the Organizational Unit 
or Root level. The SCP would explicitly deny resource creation in all regions except us-east-1 and eu-west-1 by using the aws:RequestedRegion 
condition key. If the organization is using AWS Control Tower, I would apply the SCP to the relevant Organizational Units so that both existing 
and newly created accounts automatically inherit the restriction. This approach provides centralized governance and ensures that no user, role, 
or even account administrator can create resources outside the approved regions.  

## You are managing a complex environment with Terraform. A team member manually changed a Security Group rule in the AWS Console. 
How do you identify this configuration drift, and what specific steps do you take to bring the real-world infrastructure back in sync with your code?  

I would identify the drift by running terraform plan, which compares the Terraform state and configuration with the actual AWS resources. If 
the Security Group was manually modified, Terraform will show the difference. I would validate whether the change was intentional. If it was 
unauthorized, I would run terraform apply to revert the change. If it was required, I would update the Terraform code and apply it so that the 
code remains the source of truth. I would also use CloudTrail and enforce infrastructure changes through CI/CD pipelines to minimize future 
drift.  

## You need to deploy the same VPC architecture across 10 different AWS accounts. How would you structure your Terraform modules and manage 
your backend state files to ensure the code is reusable and the state is stored securely and locked?  

To deploy the same VPC architecture across multiple AWS accounts, I would create a reusable Terraform module containing all VPC components such 
as subnets, route tables, NAT gateways, and security groups. Each account would consume the same module with different input variables like
CIDR ranges and account-specific configurations. For state management, I would use a remote S3 backend with versioning and KMS encryption 
enabled to securely store the Terraform state. I would configure DynamoDB state locking to prevent concurrent modifications and state 
corruption. Access to the backend would be controlled through IAM roles, and deployments would be performed using cross-account role 
assumption. This approach provides consistency, reusability, security, and scalable management across all AWS accounts.  

## During a terraform apply, the process crashes halfway through, leaving some resources created and others in a "tainted" state. 
How do you troubleshoot the state file to ensure the environment isn't left in a corrupted or unstabl condition?  

If Terraform crashes during an apply, I would start by running terraform plan to identify drift between the state file and actual infrastructure.
I would inspect the state using terraform state list and verify resources in AWS. Missing resources can be imported with terraform import,
stale state entries can be removed with terraform state rm, and tainted resources can be recreated using terraform apply -replace. Finally,
I would run terraform plan again to ensure the infrastructure and state are fully synchronized and stable.  

##  A third-party security auditing tool needs read-only access to your AWS environment. How would you design a secure IAM Role with Cross-Account 
access using External IDs to provide this access without sharing long-term credentials?   

I would create a cross-account IAM role with read-only permissions and configure a trust policy that allows only the third-party AWS account
to assume the role. I would use an External ID in the trust policy to protect against the confused deputy problem. The third party would use
AWS STS AssumeRole to obtain temporary credentials, eliminating the need for long-term access keys. This provides secure, auditable, and 
least-privilege access to the AWS environment.  

## An S3 bucket containing sensitive logs must be protected against accidental deletion, even by an administrator. How would you configure S3 Versioning,
MFA Delete, and Object Lock to meet this requirement?

I would enable S3 Versioning to retain all object versions, configure MFA Delete so permanent deletions require multi-factor authentication,
and enable S3 Object Lock in Compliance Mode with a defined retention period. This combination ensures that even administrators cannot 
permanently delete sensitive log files before the retention period expires, providing strong protection against accidental or malicious deletion.  

## You notice unauthorized API calls being made in a sub-account. Walk through your process of using CloudTrail and IAM Access Analyzer to track 
down the source and tighten the permissions.  

I would start with CloudTrail to identify who performed the unauthorized API calls, which credentials were used, when the activity occurred,
and from where. Once I identify the IAM user or role, I would use IAM Access Analyzer to review permissions and detect overly permissive access.
I would then apply least-privilege principles by removing unnecessary permissions, rotating compromised credentials if needed, and enabling 
continuous monitoring through CloudTrail, Access Analyzer, and Security Hub to prevent future incidents.  

## An application team reports that their EC2 instance cannot connect to an RDS database. How would you use VPC Flow Logs and CloudWatch Logs 
Insights to determine if the traffic is being rejected by a Security Group or a Network ACL?  

I would use VPC Flow Logs and CloudWatch Logs Insights to analyze traffic between the EC2 instance and the RDS database. If the flow logs show
a REJECT action, I would check the Network ACL because flow logs capture NACL decisions. If the logs show ACCEPT but the connection still fails,
I would investigate Security Group rules, ensuring the RDS Security Group allows inbound traffic from the EC2 Security Group on the required 
port. This approach helps quickly determine whether the issue is caused by a Network ACL or Security Group configuration.  

##  You need to create a dashboard that alerts the team if 4xx errors on an S3 bucket exceed a certain threshold.How would you set up the 
CloudWatch Metric Filter and Alarm to notify the team via Slack or Email?  

I would create a CloudWatch Metric Filter to capture 4xx error events from S3 access logs or CloudTrail logs and publish them as a custom 
metric. Then I would configure a CloudWatch Alarm to trigger when the error count exceeds a defined threshold. The alarm would send 
notifications through SNS, which can deliver alerts via email or integrate with Slack using a webhook or Lambda function. I would also 
add the metric to a CloudWatch Dashboard for visibility.  

## You are migrating your automation scripts from Azure TFS to GitHub Enterprise. How would you redesign a manual deployment gate in TFS into 
a GitHub Actions workflow using "Environments" and "Required Reviewers  

I would create GitHub Environments for Dev, Test, and Prod. For Production, I would configure Required Reviewers so that deployments
automatically pause before execution. The GitHub Actions workflow would deploy to the Prod environment only after approval from authorized 
reviewers. This replicates the manual deployment gate functionality from TFS while providing audit trails and controlled production releases.  

## Your Terraform pipeline in GitHub Actions needs access to AWS to deploy resources. Explain how you would use OIDC (OpenID Connect) 
instead of storing hardcoded AWS Access Keys in GitHub Secrets  

"I would configure GitHub as an OIDC identity provider in AWS and create an IAM role for Terraform deployments. The GitHub Actions workflow
would use its OIDC token to assume the IAM role through AWS STS and receive temporary credentials. This removes the need for hardcoded AWS 
access keys, improves security, and provides short-lived, least-privilege access.  

## You are asked to assist the dev team with an EKS cluster. A pod is stuck in ImagePullBackOff. How would you troubleshoot the worker node's IAM
role or the ECR repository policy to resolve this?  

If a pod is stuck in ImagePullBackOff, I would first describe the pod using kubectl describe pod <pod-name> and review the events section to 
confirm whether the failure is related to pulling the image from ECR.  

Next, I would verify that the image URI, repository name, and tag are correct and that the image exists in ECR. If the image is present,
I would check the worker node IAM role or EKS node group role. The node role should have permissions such as AmazonEC2ContainerRegistryReadOnly
or the required ECR actions including ecr:GetAuthorizationToken, ecr:BatchGetImage, and ecr:GetDownloadUrlForLayer.  

I would then verify the ECR repository policy to ensure it allows access from the EKS worker node role or account. If cross-account access is
involved, I would confirm that the repository policy explicitly grants access to the EKS account.  

Additionally, I would check network connectivity from the worker nodes to ECR, including internet access, NAT Gateway, or
VPC endpoints for ECR and S3. After correcting the IAM permissions or repository policy, I would restart the pod and verify that the image 
is successfully pulled




