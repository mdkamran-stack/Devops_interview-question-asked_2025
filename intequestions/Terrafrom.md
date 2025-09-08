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

Terraform detects manual infra changes during terraform plan as drift. We can either revert them with terraform apply or update 
code to reflect the change, but the best practice is to restrict manual changes and use automated drift detection.
