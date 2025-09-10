Infrastructure as Code (Terraform)
    What all different commands do you have in Terraform?
    How do I roll back in Terraform? Suppose I lost the infrastructure.
    What are backends in Terraform?
    What are provisioners in Terraform?
    What is data block in Terraform?
    What is the use of Terraform import command?


     Okay, code to deploy I'll give simple task only virtual machine in Azure it includes provider.tf, main.tf, locals.tf, and variables.tf. If you don't know about these files, just let me know. Can you share the screen?
    You can write all these files with the basics thing. You can take reference from Google also. But I need that code to be changed to a method where you have to variabilize that. In Google you'll get it with list of resource blocks, right? That you need to convert it to low variables and locals and other things again.

## What happens if someone manually changes infra outside of Terraform? How do you detect and fix it?

Run terraform plan → it compares state file vs actual infra and shows differences.
If manual change is unwanted → Run terraform apply → Terraform reverts infra to match code.
n
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


