Okay, code to deploy I'll give simple task only virtual machine in Azure it includes provider.tf, main.tf, locals.tf, and variables.tf. If you don't know about these files, just let me know. Can you share the screen?  


Infrastructure as Code (Terraform)
    What all different commands do you have in Terraform?  
    How do I roll back in Terraform? Suppose I lost the infrastructure.  
    What are backends in Terraform?  
    What are provisioners in Terraform?  
    What is data block in Terraform?  
    What is the use of Terraform import command?  

    # in tf how do you reference a variable from other module?  
    1. Same Module (Simple Case)

If variables are in the same module, you can reference directly:

variable "env" {
  default = "dev"
}

variable "name" {
  default = "app"
}

output "full_name" {
  value = "${var.name}-${var.env}"
}
🔹 2. Between Modules (Most Asked 🔥)

👉 Step 1: Define output in Module A

# module A (e.g., VPC)
output "vpc_id" {
  value = aws_vpc.main.id
}

👉 Step 2: Pass it to Module B

module "vpc" {
  source = "./vpc"
}

module "ec2" {
  source = "./ec2"
  vpc_id = module.vpc.vpc_id
}

👉 Step 3: Use in Module B

variable "vpc_id" {}

resource "aws_instance" "example" {
  subnet_id = var.vpc_id
}
🔹 3. Key Concept (VERY IMPORTANT)

👉

❌ You cannot directly access variables across modules
✅ You must use:
output (from source module)
module.<name>.<output> (in root module)
pass as input variable to another module

## What is the purpose of the Terraform state file?
Terraform state file is used to store the current state of infrastructure so Terraform can track resources and manage changes efficiently.  

     Okay, code to deploy I'll give simple task only virtual machine in Azure it includes provider.tf, main.tf, locals.tf, and variables.tf. If you don't know about these files, just let me know. Can you share the screen?
    You can write all these files with the basics thing. You can take reference from Google also. But I need that code to be changed to a method where you have to variabilize that. In Google you'll get it with list of resource blocks, right? That you need to convert it to low variables and locals and other things again.

## What happens if someone manually changes infra outside of Terraform? How do you detect and fix it?

Run terraform plan → it compares state file vs actual infra and shows differences.
If manual change is unwanted → Run terraform apply → Terraform reverts infra to match code.

## how do you implement cross account resource provisioning using terraform
For cross-account provisioning, I usually configure Terraform to assume IAM roles in the target accounts. I use multiple provider blocks with aliases, and sometimes remote state data sources to share outputs between accounts.

## Accidently deleted the state file.
Always store state file in a remote backend eg.S3 with Dynamodb and then eanble versioning.

## how do you handle terraform state file corruption?
If the state file is corrupted, I first restore from the backup or remote backend version. If that fails, I use terraform import to
rebuild state from existing resources

## Two people running terraform apply at the same time 
Enable state locking so you dont overwrite.

## how do you manage secrest in terraform without hardcoding

We have Integrate with AWS Secrets Manager, SSM Parameter Store ,Terraform fetched secrets dynamically at runtime.

## Terraform apply fails hilfway?
Use terrfaorm plan first and then run terrfaorm apply -refresh=true to sync the state.

## AWS API rate limit errors.
Configure retries in your provider and stagger deployment to avoid hitting limit.

## Infrastructure drift (code vs actual cloud state)
run terrafrom plan regulary 

## Resource removed from code but still exist in cloud 
Use terrfarom destroy -target 

## Hitting AWS quota limits.
Keep an eye on quotas and request increase before scaling events.

## lost access to remote backend 
Document access procedure and keep secure backups of state file.

1) What is the difference between 𝐭𝐞𝐫𝐫𝐚𝐟𝐨𝐫𝐦 𝐢𝐦𝐩𝐨𝐫𝐭 and 𝐭𝐞𝐫𝐫𝐚𝐟𝐨𝐫𝐦 𝐭𝐚𝐢𝐧𝐭?
2) How do you manage secrets in Terraform without hardcoding them?
3) What’s the difference between 𝐜𝐨𝐮𝐧𝐭 and 𝐟𝐨𝐫_𝐞𝐚𝐜𝐡? Give a real-world use case.
4) How do you handle drift detection in Terraform?
5) What is a Terraform remote backend, and why is it important?
6) How do you manage multiple environments (dev, staging, prod) in Terraform?
7) Difference between 𝐥𝐨𝐜𝐚𝐥-𝐞𝐱𝐞𝐜 and 𝐫𝐞𝐦𝐨𝐭𝐞-𝐞𝐱𝐞𝐜 provisioners.
8) How do you safely roll back infrastructure changes after a failed deployment?
9) Explain 𝐭𝐞𝐫𝐫𝐚𝐟𝐨𝐫𝐦 𝐫𝐞𝐟𝐫𝐞𝐬𝐡 vs 𝐭𝐞𝐫𝐫𝐚𝐟𝐨𝐫𝐦 𝐩𝐥𝐚𝐧.
10) How do you write reusable Terraform modules?


