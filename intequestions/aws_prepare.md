# Ec2 Instance required Internet access for package update 


1 create natgateway attach to public subnet with your vpc & Allocate ealstic ip 

3 Edit route table of private subnet add route 0.0.0.0 target should be natgateway 

make sure public subent as route to IGW  whereas private subnet has route to natgateway 

# How to migrate :  hot migration migration on running mc   cold migration off the m/c do the migration
# migration of DB on-perm to aws 
## Correct DMS Migration Flow (On-premises → AWS RDS)  
## Create the target RDS instance (MySQL, PostgreSQL, SQL Server, etc.).
## Ensure network connectivity between the on-premises database and AWS (VPN, Direct Connect, or internet if appropriate).
## Create a DMS Replication Instance.
## Create the source endpoint (on-premises database).
## Create the target endpoint (AWS RDS).
## Test endpoint connections.
## Create a DMS migration task:
Full load only
Full load + CDC (Change Data Capture) for minimal downtime
CDC only
## Start the migration task.
## Validate the migrated data.
## Cut over the application to the RDS instance once migration is complete.

# can you explain how you migrated an on-premises VM to AWS?

## Step 1: Assessment

"First, I identified the source server details such as the operating system, CPU, memory, disk usage, IP address, installed applications, and dependencies. I also verified network connectivity from the on-premises environment to AWS."

## Step 2: Prepare AWS MGN

"In AWS, I opened Application Migration Service, selected the target AWS Region, and configured the replication settings, including the staging area subnet and IAM roles."

## Step 3: Install the Replication Agent

"On the source Linux server, I downloaded and installed the AWS Replication Agent using the installation script. During installation, I provided the AWS Region so the server could register with AWS MGN."

Example:

sudo ./aws-replication-installer-init
## Step 4: Start Replication

"Once the agent was installed, it performed an initial full disk replication to the staging area in AWS. After that, it continuously replicated only changed blocks until the server was ready for cutover."

## Step 5: Monitor Replication

"I monitored the replication progress in the AWS MGN console until the server reached a healthy and ready state."

## Step 6: Configure Launch Settings

"Before launching the migrated server, I configured the target EC2 settings such as:

Instance type
VPC
Subnet
Security Group
IAM Role
EBS volume configuration
Key Pair"
## Step 7: Launch a Test Instance

"I launched a test instance first to verify the migration without impacting production."

## Step 8: Validate

I verified:

SSH access
Application services
Mounted file systems
Disk sizes
CPU and memory
Network connectivity
Logs
Application functionality
## Step 9: Perform Cutover

"After successful testing, I scheduled a maintenance window, stopped the source application, allowed final replication to complete, and launched the cutover instance."

## Step 10: Post-Migration Validation

I verified:

Application accessibility
Database connectivity
DNS updates (if required)
User access
Monitoring and CloudWatch
Backups  

# tf module for vpc 
# modules/vpc/variables.tf

provider "aws" {  
  region = "ap-south-1"  
}  

# VPC  
resource "aws_vpc" "main" {  
  cidr_block = "10.0.0.0/16"  

  tags = {  
    Name = "my-vpc"    
  }  
}  

# Internet Gateway  
resource "aws_internet_gateway" "main" {  
  vpc_id = aws_vpc.main.id  

  tags = {  
    Name = "my-igw"  
  }  
}  

# Public Subnet    
resource "aws_subnet" "public" {  
  vpc_id                  = aws_vpc.main.id   
  cidr_block              = "10.0.1.0/24"  
  map_public_ip_on_launch = true  

  tags = {  
    Name = "public-subnet"  
  }  
}  

# Private Subnet  
resource "aws_subnet" "private" {  
  vpc_id     = aws_vpc.main.id  
  cidr_block = "10.0.2.0/24"  

  tags = {  
    Name = "private-subnet"  
  }  
}  

# Public Route Table  
resource "aws_route_table" "public" {  
  vpc_id = aws_vpc.main.id  

  route {   
    cidr_block = "0.0.0.0/0"  
    gateway_id = aws_internet_gateway.main.id  
  }  

  tags = {  
    Name = "public-route-table"  
  }  
}  

# Public Subnet - Route Table Association  
resource "aws_route_table_association" "public" {  
  subnet_id      = aws_subnet.public.id  
  route_table_id = aws_route_table.public.id  
}  

# EC2 instance_TF_File 

data "aws_ami" "amazon_linux" {
  most_recent = true

  owners = ["amazon"]

  filter {
    name   = "name"
    values = ["al2023-ami-*-x86_64"]
  }

  filter {
    name   = "state"
    values = ["available"]
  }
}

Example EC2 resource:

resource "aws_instance" "web" {
  ami           = data.aws_ami.amazon_linux.id
  instance_type = var.instance_type
  subnet_id     = var.subnet_id

  tags = {
    Name = "web-server"
  }
}

Interview Explanation

I avoid hardcoding AMI IDs. I use the aws_ami data source to dynamically find the required Amazon Linux AMI. In an enterprise environment, if the organization uses a golden AMI, I would pass the approved AMI ID as a variable instead.

# S3 Bucket Creation

Folder Structure

terraform/
└── modules/
    └── s3/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf

# modules/s3/variables.tf

variable "bucket_name" {
  type = string
}

variable "environment" {
  type = string
}

# modules/s3/main.tf

S3 Bucket

resource "aws_s3_bucket" "this" {
  bucket = var.bucket_name

  tags = {
    Name        = var.bucket_name
    Environment = var.environment
  }
}

Versioning

resource "aws_s3_bucket_versioning" "this" {
  bucket = aws_s3_bucket.this.id

  versioning_configuration {
    status = "Enabled"
  }
}

Server-Side Encryption

resource "aws_s3_bucket_server_side_encryption_configuration" "this" {
  bucket = aws_s3_bucket.this.id

  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

Public Access Block

resource "aws_s3_bucket_public_access_block" "this" {
  bucket = aws_s3_bucket.this.id

  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}

# modules/s3/outputs.tf

output "bucket_id" {
  value = aws_s3_bucket.this.id
}

output "bucket_arn" {
  value = aws_s3_bucket.this.arn
}

# Example Root Module

module "s3" {
  source = "./modules/s3"

  bucket_name = "my-interview-bucket-12345"
  environment = "prod"
}

# How would you set up an EKS cluster?"

I would provision the EKS infrastructure using Terraform. First, I create a multi-AZ VPC with public and private subnets. The EKS control plane is AWS-managed, while worker nodes are deployed in private subnets. I configure IAM roles for the EKS cluster and node groups, security groups, and the required networking. Then I create the EKS cluster, add managed node groups, configure the AWS VPC CNI, CoreDNS and kube-proxy, and configure access using IAM and Kubernetes RBAC. Finally, I install the AWS Load Balancer Controller, configure monitoring and logging, and validate cluster connectivity and workloads."
