Below services must have clear understanding check KR NEtwork video   
1: EC2  - run apps   2: S3 -sore file    3: IAM - manage access   4: RDS  - databases    5: LAMBDA - run code without server   6: cloudwatch  -Monitor     7: VPC - Network setup  

## Bash one-liner: Delete all .tmp files older than 15 days in /var/log

find /var/log -type f -name "*.tmp" -mtime +15 -exec rm -f {} \;

## what is different bw a name vs cname records.

## A (Address Record) – Maps a domain name to an IPv4 address. eg:ec2 instance , lambda

blog.dnsimple.com.     A        185.31.17.133

## CNAME Record (Canonical Name Record) Alias record that maps one domain to another.

Purpose: Maps a domain/subdomain → another domain name (not an IP)   EG:  app.example.com → myapp-alb-1234.elb.amazonaws.com

**NS record is responsible for resoving our domain.**  

## How do you ensure that your infrastructure is scalable and can handle unexpected traffic spikes?

in one project our e-commerce app faced traffic spikes during sales. We used Kubernetes HPA to scale Pods based on CPU/memory and AWS ALB to distribute traffic. We also cached frequent queries in Redis. This allowed the system to handle a 3x spike in traffic smoothly without downtime.

## EBS vs EFS vs FSx. walk through use cases and tradeoffs.

EBS is block storage attached to one EC2, best for databases or boot volumes. EFS is a managed NFS file system that can be mounted by many instances across AZs, good for shared storage like Jenkins or CMS. FSx provides specialized file systems like Windows FS (SMB), Lustre for HPC, and ONTAP for enterprise workloads. I usually pick EBS for single-instance apps, EFS for shared Linux workloads, and FSx when I need Windows or HPC/enterprise-grade storage

## what is purpose of NAT gateway
A NAT Gateway allows instances in a private subnet to access the internet (for updates, package downloads, etc.) without exposing them to inbound internet traffic.  

## Your EC2 instance in a private subnet needs to download packages wihout nat gateway , what alternative exist.

Launch EC2 in a Private Subnet

Place the EC2 instance in a private subnet (no public IP assigned).

This ensures it cannot be accessed directly from the internet.

Create a NAT Gateway / NAT Instance

Deploy a NAT Gateway in a public subnet.

Update the private subnet’s route table so outbound internet traffic (0.0.0.0/0) goes via the NAT Gateway.

This way, EC2 can reach the internet outbound only (for yum/apt/pip updates etc.), but inbound traffic is blocked.

Restrict Security Groups

Allow only required inbound ports (e.g., 22 for SSH but only from a bastion host OR from a VPN).

Outbound can remain open (default) to allow package downloads.

Use a Bastion Host (Optional)

If you need SSH/RDP, launch a bastion host in the public subnet.

Access EC2 from the bastion host using private IPs (via SSH agent forwarding or AWS SSM).

(Best Practice) Use AWS SSM Session Manager

Instead of bastion host/SSH, use AWS Systems Manager Session Manager to securely connect to the instance without opening port 22 at all.

If I can’t use a NAT Gateway, I’d use VPC Endpoints for S3/SSM or set up a proxy in a public subnet. This allows private EC2s to get packages without exposing them to the internet.

## You have a application in account A that needs to access an S3 bucket in Account B how would you configure it?

I’d configure a bucket policy in Account B to trust the IAM role from Account A, and then grant that role S3 permissions. Optionally,
I can set up a cross-account role in Account B and let Account A assume it for tighter security

## What happens in an S3 PUT call from CLI to data persistence?

S3 PUT from CLI authenticates your request, transfers data securely, and stores it durably across AZs with optional triggers and metadata indexing.

## how do you implemet drift detection across infra environments  

I implement drift detection by integrating IaC tools like Terraform or CloudFormation with CI/CD pipelines, audit logs, and monitoring platforms to continuously compare desired and actual infrastructure states across environments.


