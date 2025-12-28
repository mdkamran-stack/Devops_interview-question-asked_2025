Below services must have clear understanding check KR NEtwork video   
1: EC2  - run apps   2: S3 -sore file    3: IAM - manage access   4: RDS  - databases    5: LAMBDA - run code without server   6: cloudwatch  -Monitor     7: VPC - Network setup  

## Bash one-liner: Delete all .tmp files older than 15 days in /var/log

find /var/log -type f -name "*.tmp" -mtime +15 -exec rm -f {} \;

## what is different bw a name vs cname records.

An A record maps a domain directly to an IP address, whereas a CNAME record maps a domain to another domain name.

## A (Address Record) – Maps a domain name to an IPv4 address. eg:ec2 instance , lambda

blog.dnsimple.com.     A        185.31.17.133

## CNAME Record (Canonical Name Record) Alias record that maps one domain to another.

Purpose: Maps a domain/subdomain → another domain name (not an IP)   EG:  app.example.com → myapp-alb-1234.elb.amazonaws.com

**NS record is responsible for resoving our domain.**  

## what is vpc

**A VPC is a private, isolated network in AWS where we can securely launch resources, control IP ranges, subnets, routing, and firewall rules, similar to having our own data center inside AWS.**

# Key Features of a VPC

- **Isolation** → Your own private network inside AWS.  
- **Subnets** → Divide network into public (internet-facing) and private (internal) subnets.  
- **Route Tables** → Control traffic flow between subnets and internet.  
- **Internet Gateway (IGW)** → Allows resources in public subnet to connect to the internet.  
- **NAT Gateway** → Lets private subnet instances reach the internet without being exposed.  
- **Security Groups & NACLs** → Firewall-like features for controlling inbound and outbound traffic.  
- **Peering & Transit Gateway** → Connect VPCs with each other securely.  


## How do you ensure that your infrastructure is scalable and can handle unexpected traffic spikes?

in one project our e-commerce app faced traffic spikes during sales. We used Kubernetes HPA to scale Pods based on CPU/memory and AWS ALB to distribute traffic. We also cached frequent queries in Redis. This allowed the system to handle a 3x spike in traffic smoothly without downtime.

## EBS vs EFS vs FSx. walk through use cases and tradeoffs.

EBS is block storage attached to one EC2, best for databases or boot volumes. EFS is a managed NFS file system that can be mounted by many instances across AZs, good for shared storage like Jenkins or CMS. FSx provides specialized file systems like Windows FS (SMB), Lustre for HPC, and ONTAP for enterprise workloads. I usually pick EBS for single-instance apps, EFS for shared Linux workloads, and FSx when I need Windows or HPC/enterprise-grade storage

## what is purpose of NAT gateway
A NAT Gateway allows instances in a private subnet to access the internet (for updates, package downloads, etc.) without exposing them to inbound internet traffic.  

## Your EC2 instance in a private subnet needs to download packages wihout nat gateway , what alternative exist.

# Steps to Explain: Securely Access Packages from EC2 Without Public Exposure

## 1. Launch EC2 in a Private Subnet
- Place the EC2 instance in a **private subnet** (no public IP assigned).  
- This ensures it cannot be accessed directly from the internet.  

## 2. Create a NAT Gateway / NAT Instance
- Deploy a **NAT Gateway** in a public subnet.  
- Update the private subnet’s **route table** so outbound internet traffic (`0.0.0.0/0`) goes via the NAT Gateway.  
- This way, EC2 can reach the internet outbound only (for `yum`/`apt`/`pip` updates etc.), but inbound traffic is blocked.  

## 3. Restrict Security Groups
- Allow only required inbound ports (e.g., **22 for SSH**, but only from a bastion host OR from a VPN).  
- Outbound can remain open (default) to allow package downloads.  

## 4. Use a Bastion Host (Optional)
- If you need SSH/RDP, launch a **bastion host** in the public subnet.  
- Access EC2 from the bastion host using **private IPs** (via SSH agent forwarding or AWS SSM).  

## 5. (Best Practice) Use AWS SSM Session Manager
- Instead of bastion hosts, use **AWS Systems Manager Session Manager** for secure, audited, and agent-based access to private EC


## You have a application in account A that needs to access an S3 bucket in Account B how would you configure it?

I’d configure a bucket policy in Account B to trust the IAM role from Account A, and then grant that role S3 permissions. Optionally,
I can set up a cross-account role in Account B and let Account A assume it for tighter security

## What happens in an S3 PUT call from CLI to data persistence?

S3 PUT from CLI authenticates your request, transfers data securely, and stores it durably across AZs with optional triggers and metadata indexing.

## how do you implemet drift detection across infra environments  

I implement drift detection by integrating IaC tools like Terraform or CloudFormation with CI/CD pipelines, audit logs, and monitoring platforms to continuously compare desired and actual infrastructure states across environments.

## aws how to create highly available and secure app

We design the app across multiple AZs with an internet-facing ALB and Auto Scaling groups for app servers in private subnets, connect to an RDS Multi-AZ database and use S3 + CloudFront for static assets. Security is enforced with least-privilege IAM roles, Security Groups, TLS via ACM, secrets stored in Secrets Manager, and WAF/GuardDuty for threat detection. We automate provisioning and deployments with Terraform and CI/CD pipelines, use blue/green or canary deployments for safe releases, and ensure observability with CloudWatch, X-Ray, and centralized logging — all together providing high availability, resilience, and strong security posture.

## How to deploy a services.

1: we will build the application docker image  
2: create the infrastrucuture servers 
3: configuring dependencies  Env variable , storage  
4: Deploy the services pull the container images  
5: Expose the services Configure load balancers, DNS, firewalls, and service discovery. 
6: Monitor and validate using prometehus & Grafana  
## In kubernetes 
First we will create a deployment.yaml file & kubect apply -f deployment.yaml  
then we will deploy the services.yaml file & kubectl appy -f deployment.yaml 
kubectl get pod & svc for cross verifications  

## In inerview asked me >> do you have interaction of databse team? are you involve in any cutover ?

Yes, I collaborate with the database team for schema changes and data migrations. I’ve also participated in cutovers, coordinating deployments, migrations, 
and monitoring to ensure a smooth transition to the new system.

