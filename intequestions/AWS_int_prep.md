## Bash one-liner: Delete all .tmp files older than 15 days in /var/log

find /var/log -type f -name "*.tmp" -mtime +15 -exec rm -f {} \;

## what is different bw a name vs cname records.

## A (Address Record) – Maps a domain name to an IPv4 address. eg:ec2 instance , lambda

blog.dnsimple.com.     A        185.31.17.133

## CNAME Record (Canonical Name Record) Alias record that maps one domain to another.

Purpose: Maps a domain/subdomain → another domain name (not an IP)   EG:  app.example.com → myapp-alb-1234.elb.amazonaws.com

## NS record is responsible for resoving our domain.  

## EBS vs EFS vs FSx. walk through use cases and tradeoffs.

EBS is block storage attached to one EC2, best for databases or boot volumes. EFS is a managed NFS file system that can be mounted by many instances across AZs, good for shared storage like Jenkins or CMS. FSx provides specialized file systems like Windows FS (SMB), Lustre for HPC, and ONTAP for enterprise workloads. I usually pick EBS for single-instance apps, EFS for shared Linux workloads, and FSx when I need Windows or HPC/enterprise-grade storage

## What happens in an S3 PUT call from CLI to data persistence?

S3 PUT from CLI authenticates your request, transfers data securely, and stores it durably across AZs with optional triggers and metadata indexing.

## how do you implemet drift detection across infra environments  

I implement drift detection by integrating IaC tools like Terraform or CloudFormation with CI/CD pipelines, audit logs, and monitoring platforms to continuously compare desired and actual infrastructure states across environments.


