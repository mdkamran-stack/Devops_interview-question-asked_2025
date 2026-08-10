# Ec2 Instance required Internet access for package update 


1 create natgateway attach to public subnet with your vpc & Allocate ealstic ip 

3 Edit route table of private subnet add route 0.0.0.0 target should be natgateway 

make sure public subent as route to IGW  whereas private subnet has route to natgateway 

# Instance types
General purpose T2 M5 M4 M3  
Memory optimized x1e X1 R4 R3  
Storage opt h1 i3 D2  
Accelarated computing P3 P2 G3 F1    
Compute opt C5 C4 C3   


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

# Aws Cost Optimization:

1> Selecting right size of EC2 instance based on upon cpu & memory utilization of application workload we selct rigt size of insatnce if we have predictable workload if we are planning 
to use application one yeayr or more than that we will go for reserved instance which provide upto 75% discount compare to ondemand insatnces.

2> we use spot instance for non production workload   
3> In my curent project we have microservices based application which runs on eks cluster we are using aws eks in eks we can also follow some cost optimization startegies like we are using clsuter autoscaler that is nothing but autoscaling group itself promte cost optimzation because when running instances whenever needed along with that define right size of cpu & memory limits in my clsuter   
4> followed but when it comes to stoarage i can also do the cost optimization by moving the data from one storage to another storage by using S3 lifecycle policy by deleting unused snapshort and EBS volume .  
5> when it comes to networking instead of using nat gateway we can use vpc endpoints within the aws communication if it goes through vpc endpoint we avoid the charge of nat & data transfer cost.    
6> we use right size of database instances   
7> we have some native services in aws aws cost oxplorer i can track daily weekly and montly costing of my resources i can create some budget and cost anamoly alert if it reaches we get an alert .  

# Disaster recovery setup
In Production we run the application in a primary aws region and maintain DR environment in a secondary Region we use aws backup for scheduled backup and cross region copies, with encryption using KMS , for critical application we maintain a warm standy or multi site DR environment , route53 monitors the primary endpoint using health check s and if the primary Region failes DNS traffic is automatically failed over to the DR Region we define RTO and RPO based on business requirements .

# Cross account Account A wants to access account B S3 access  

1. Create an IAM role in Account B

The role has a trust policy allowing Account A to assume it.  

2. Give the role S3 permissions
   
I attach least-privilege S3 permissions to that role.

3. Account A assumes the role

Account A uses AWS STS AssumeRole to obtain temporary credentials and access the S3 bucket in Account B.

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

AWS Provider block 

provider "aws" { 
region = "ap-south-1"  

}  

Find Latest Amzon AMI  

data"aws_ami" "amazon_linux" {   
most_recent =true  

owners = ["amazon"]  

filter {  
name = "name"  
values = ["ami-*86_64"]  
}  

fileter {  
name = "state"  
values = ["availability"]  
}  
}  

# 2. Deploy instance using the dynamic AMI id   
resource "aws_instacne" "web" {  
ami = data.aws_ami linux.id  
instacne_type "t3.micro"  
subnet_id = var.subnet_id  

tags = {  
Name = "web_server"  
}  
}  

# Varibale.tf 

variable "instance_type" {  
type = string    
default ="t3.micro"  
}  

varibale "subnet_id" {  
type = string   
}  

varibale "instance_name" { 
type =string   
default = "web-server"  
}  

# output.tf  

output "instance_id" {
  value = aws_instance.this.id
}

output "private_ip" {
  value = aws_instance.this.private_ip
}
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

# Jenkins Declarative CI/CD Pipeline

```groovy
pipeline {
    agent any

    environment {
        IMAGE = "myapp"
        REGISTRY = "docker.io/myrepo"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/myorg/myapp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $REGISTRY/$IMAGE:$BUILD_NUMBER .'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $REGISTRY/$IMAGE:$BUILD_NUMBER'
            }
        }

        stage('Deploy') {
            steps {
                sh 'kubectl set image deployment/myapp myapp=$REGISTRY/$IMAGE:$BUILD_NUMBER'
            }
        }
    }

    post {
        success {
            echo 'Deployment successful'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}

# if we have to show env variable 
stage('Build') {
    environment {
        ENV = "dev"
    }

    steps {
        sh 'echo $ENV'
    }
}
```

# Multi-Stage Dockerfile for Node.js

```dockerfile
# Stage 1: Build

FROM node:22-alpine AS build

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build


# Stage 2: Production

FROM node:22-alpine AS production

WORKDIR /app

ENV NODE_ENV=production

COPY package*.json ./

RUN npm ci --omit=dev

COPY --from=build /app/dist ./dist

EXPOSE 3000

CMD ["node", "dist/index.js"]
```

