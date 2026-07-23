# This commit is for questions asked me as devops interview in 2025  

# Trinet

## What is multistage docker file why it is used. 
A multi-stage Dockerfile uses multiple FROM statements in a single Dockerfile to separate the build environment from the runtime environment.
## Setup build application multibrach java app and load the artifactory  

## Autobuild jenkins pipeline  
We automate the Jenkins pipeline using a GitHub webhook. Whenever a developer pushes code, Jenkins automatically triggers the pipeline, checks out the code, builds the application, runs tests, performs code quality and security scans, builds a Docker image, pushes it to ECR/Nexus, and deploys it to Kubernetes using Helm or Argo CD. Finally, it sends a success or failure notification

## How to push repository in nexus  
Method 2: Upload Using cURL 
curl -u username:password \
--upload-file app.jar \
http://nexus.company.com/repository/maven-releases/app.jar

## Jenkins jobs running almost 2 hours jobs start normally. 
5-Step Troubleshooting
Check Console Output – Identify the stage where the job is stuck.
Check Jenkins Agent – Ensure the agent is online and responsive.
Check Resources – Verify CPU, memory, and disk (top, free -h, df -h).
Check External Dependencies – Git, Nexus, Docker, SonarQube, Kubernetes, cloud services.
Check Jenkins Logs – Review logs and compare with the last successful build.

## aws secret can't fetched how do you debug.  
5-Step Debug Process (Easy to Remember)
Check application logs – Identify the exact error.
Verify IAM Role/IRSA – Ensure secretsmanager:GetSecretValue is allowed.
Verify the secret – Confirm the secret name and AWS Region are correct.
Check AWS identity – Run aws sts get-caller-identity.
Check networking & KMS – Verify NAT/VPC Endpoint and kms:Decrypt permission if using a customer-managed KMS key.

# Dish Network 

## what is shared library  in jenkins 
A Jenkins Shared Library is a reusable collection of Groovy scripts, functions, and pipeline code that can be shared across multiple Jenkins pipelines.

## what is db sharding 
Database sharding is a horizontal scaling technique where a large database is split into multiple smaller databases, called shards. Each shard stores a portion of the data, which distributes read and write traffic across multiple servers, improving performance and scalability. 

## dockerfile file for nodejs app
## lets consider got a req from custom docker image private repo when building docker image in internal image & push to private artifact how to integrate base image push private registry?
## I have a 100 docker cont running lot of uns=used container cleaning ?
## What are the k8s cluster type? 
There are two main types of Kubernetes clusters: self-managed and managed. In self-managed clusters, the organization manages the control plane and worker nodes using tools like kubeadm or OpenShift. In managed clusters, cloud providers manage the control plane, such as Amazon EKS, Azure AKS, and Google GKE. Depending on business requirements, these clusters can also be deployed in on-premises, hybrid, or multi-cloud environments.
## EKS addons 
## what is k8s webhooks & its types 
Kubernetes Webhooks are admission controllers that intercept API requests before they are persisted. There are two types: Mutating Admission Webhooks, which modify resources such as injecting sidecars or adding labels, and Validating Admission Webhooks, which enforce policies by allowing or rejecting requests. Mutating webhooks run first, followed by validating webhooks 
## How do you update an application without disturbing the existing running Pods? 
I use Kubernetes Rolling Updates. When I deploy a new image, Kubernetes creates new Pods gradually while keeping the existing Pods running. Only after the new Pods pass their readiness probe and become healthy does Kubernetes terminate the old Pods. This ensures zero downtime and no impact on users. 
### maxSurge: 1 → Create one extra Pod during the update.
### maxUnavailable: 0 → Never make an existing Pod unavailable until a new Pod is ready.

## when you have images docker file image is stored in nexus to download image how to manage authentication and put to k8s 
When Docker images are stored in a private Nexus registry, Kubernetes must authenticate before pulling the image. We create a Docker registry Secret (imagePullSecret) with the Nexus credentials and reference it in the Pod or ServiceAccount. During deployment, Kubernetes uses the imagePullSecret to authenticate with Nexus, pull the image, and start the Pod.
## in a node created deployment yaml run kubectl apply 4 pod all 4 split into 4 worker node like pod1 got to node 1
## 

# Ispace

how to integrate load testing in cicd pipeline  
# what is deployment and daemon set .  
A Deployment is used to manage stateless applications by maintaining the desired number of replicas and supporting rolling updates and rollbacks. A DaemonSet ensures that a copy of a Pod runs on every worker node, making it ideal for node-level services such as log collectors, monitoring agents, and networking components. In production, we use Deployments for applications and DaemonSets for infrastructure services.

# what is ingress how to use .  
Ingress is a Kubernetes resource that manages external HTTP/HTTPS access to applications running inside the cluster. It routes incoming traffic to the appropriate Service based on the hostname or URL path.

# two module a & b moudle a is creating ec2 instance module instance id how to to get instance id by module b 

To share the EC2 instance ID between Terraform modules, I expose the instance ID as an output from Module A using an output block. Then, in the root module, I pass module.ec2.instance_id as an input variable to Module B. This keeps the modules loosely coupled and follows Terraform best practices.

# Pepsico
==========
### What is output of Maven Build, if it is jar file or war file what command will put in docker file?
### scenario: you have one repo and want to build a pipeline to deploy to AWS (e.g., ECS, EKS, 
### how is your application is connected in frontend in your cluster 
### authentication and autherization who will manage application or loadbalancer ? 
### how it will authenticate weather user is valid or not  
### what is HIGH IO usage  
### how to export metrics to prometeus? 
###  lets say you have application java running micro services in kubernetes cluster from there to grafana
### 3 instance in 3 diff regions 
### 3 ec3 instance load from external source i have to divide traffic equally on all 3 
### git stash 

# Bristlecone interview
------------------------
### create 10 ubnut server with apache installations  /var/lib/html via terraform   
### write shell script get the process with user name kamran and kill the process   
### how to deploy application in k8s     
In production, the deployment process starts when a developer pushes code to Git. A CI tool such as Jenkins or builds and tests the application, creates a Docker image, and pushes it to a container registry like ECR. A CD tool such as Argo CD then deploys the updated Kubernetes manifests or Helm chart to the cluster. Kubernetes creates or updates the Deployment, which manages ReplicaSets and Pods. A Service exposes the Pods internally, and an Ingress or LoadBalancer exposes the application externally. After deployment, I verify the rollout using kubectl rollout status and monitor the Pods and application health
### how to setup k8s in eks    
> I start by creating a VPC with public and private subnets across multiple Availability Zones. Then I create the EKS cluster and managed node groups using `eksctl` or Terraform. After configuring `kubectl`, I install essential add-ons such as the VPC CNI, CoreDNS, kube-proxy, AWS Load Balancer Controller, Metrics Server, EBS CSI Driver, and Cluster Autoscaler. Finally, I deploy applications, configure monitoring with Prometheus and Grafana, enable autoscaling, and secure the cluster using IAM roles, RBAC, and AWS Secrets Manager.
### types of loadbalancer in k8s    
 Kubernetes Service Types

| Service Type | Accessible From | Use Case |
|--------------|-----------------|----------|
| ClusterIP | Inside the cluster | Internal communication |
| NodePort | Outside via Node IP and Port | Testing and development |
| LoadBalancer | Internet (Cloud Provider) | Production applications |
| ExternalName | External DNS | External services |
### how to setup hpa na vpa    
HPA (Horizontal Pod Autoscaler)

Purpose: Automatically scales the number of Pods based on metrics such as CPU, memory, or custom metrics.

Example
kubectl autoscale deployment nginx \
  --cpu-percent=70 \
  --min=2 \
  --max=10

Or apply an HPA YAML.

Use case:

If CPU usage exceeds 70%, Kubernetes increases the number of Pods.
When CPU usage drops, it scales the Pods down.

VPA (Vertical Pod Autoscaler)

Purpose: Automatically adjusts the CPU and memory requests/limits of Pods instead of changing the number of Pods.

Example VPA
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: myapp-vpa
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: myapp
  updatePolicy:
    updateMode: Auto

Use case:

If a Pod needs more memory, VPA increases its CPU/memory requests.
It may recreate Pods to apply the new resource settings.

### what is selinux    
### what is loadbalncer in aws & how to setup app LB and NW LB , how network LB works    
How to Set Up an Application Load Balancer (ALB)

## Step 1: Launch EC2 Instances

Deploy at least **2 EC2 instances** in different Availability Zones.

```
EC2-1 (AZ-1)
EC2-2 (AZ-2)
```

---

## Step 2: Create a Target Group

- Target Type: Instance
- Protocol: HTTP
- Port: 80
- Register EC2 instances

---

## Step 3: Create an ALB

- Scheme: Internet-facing
- Type: Application Load Balancer
- Select at least two public subnets
- Attach Security Group
- Attach Target Group

---

## Step 4: Configure Listener

```
HTTP :80
```

or

```
HTTPS :443
```

Forward traffic to the Target Group.

---

## Step 5: Configure Health Check

Example:

```
Path:
/health
```

Healthy targets receive traffic.

---

## ALB Traffic Flow

```
Internet
      │
      ▼
Application Load Balancer
      │
      ▼
Target Group
      │
 ┌────┴────┐
 ▼         ▼
EC2-1    EC2-2
```

---

# How to Set Up a Network Load Balancer (NLB)

## Step 1

Launch EC2 instances.

---

## Step 2

Create a Target Group

- Protocol: TCP
- Port: 8080 (example)

---

## Step 3

Create Network Load Balancer

- Internet-facing or Internal
- Select subnets
- Attach Target Group

---

## Step 4

Create Listener

```
TCP :8080
```

or

```
TLS :443
```

---

## Step 5

Verify Health Checks

```
TCP
```

or

```
HTTP
```

---
### What is the difference between the count and for_each meta-arguments?    
> Both **`count`** and **`for_each`** are Terraform meta-arguments used to create multiple resources. The key difference is that **`count`** creates resources using a numeric index, while **`for_each`** creates resources using unique keys from a map or set. I use **`count`** when all resources are nearly identical, and **`for_each`** when each resource has unique attributes or names. In production, `for_each` is generally preferred because it provides stable resource addressing and avoids unnecessary resource recreation when items are added or removed.

---

# count

- Uses a numeric index (`0,1,2...`)
- Best when resources are identical
- Access resources using `count.index`

### Example

```hcl
resource "aws_instance" "web" {
  count = 3

  ami           = "ami-123456"
  instance_type = "t3.micro"

  tags = {
    Name = "server-${count.index}"
  }
}
```

Creates:

```
server-0
server-1
server-2
```

---

# for_each

- Uses a **map** or **set**
- Each resource has a unique key
- Access values using `each.key` and `each.value`

### Example

```hcl
resource "aws_instance" "web" {
  for_each = {
    web1 = "t3.micro"
    web2 = "t3.small"
    web3 = "t3.medium"
  }

  ami           = "ami-123456"
  instance_type = each.value

  tags = {
    Name = each.key
  }
}
### how to communicate with two infrastructure?
> The communication method depends on where the infrastructures are hosted. If both are AWS VPCs, I use **VPC Peering** or **AWS Transit Gateway**. If communication is between on-premises and AWS, I use **Site-to-Site VPN** or **AWS Direct Connect**. For communication between different cloud providers such as AWS and Azure, I use VPN or Direct Connect/ExpressRoute. After connectivity is established, I configure route tables, security groups, and network ACLs to allow only the required traffic.

---

# Common Communication Methods

| Scenario | Solution |
|----------|----------|
| AWS VPC to AWS VPC | VPC Peering |
| Multiple AWS VPCs | AWS Transit Gateway |
| AWS to On-Premises | Site-to-Site VPN |
| AWS to On-Premises (High Bandwidth) | AWS Direct Connect |
| AWS to Azure | VPN or Direct Connect + ExpressRoute |
| Kubernetes Cluster to Kubernetes Cluster | VPN, Transit Gateway, Service Mesh |
| Application to Database | Private Subnet + Security Groups |

---

### how to migrate on-perm to cloud?
I begin by assessing the existing on-premises environment, including applications, databases, dependencies, and network architecture. Based on the assessment, I select an appropriate migration strategy such as Rehost or Replatform. I provision the AWS infrastructure using Terraform, establish secure connectivity with a Site-to-Site VPN or Direct Connect, migrate servers using AWS MGN, migrate databases using AWS DMS, and transfer files using DataSync or Snowball when needed. After thorough testing, I perform the production cutover during a maintenance window, monitor the application with CloudWatch, and decommission the on-premises infrastructure once the migration is confirmed to be successful.

### how to communicate microservices?
 Microservices communicate either synchronously using REST APIs or gRPC, or asynchronously using messaging systems such as Kafka, RabbitMQ, Amazon SQS, or Amazon SNS. In Kubernetes, services communicate internally through Kubernetes Services and DNS. In production, REST/gRPC is typically used for request-response communication, while message queues are used for event-driven workloads to improve scalability and reliability.

# QuessCorp
------------------
### Terraform list vs terraform show  
### IN CI/CD pipeline how to configure that pipeline run during change window and deploy app in cluster  
1. Jenkins (Most Common)

Use an input step for CAB/change approval and a time check before deployment.

stage('Deploy') {
    steps {
        script {
            def hour = new Date().format("HH", TimeZone.getTimeZone('Asia/Kolkata')) as Integer

            if (hour >= 22 && hour <= 23) {
                sh 'helm upgrade --install myapp ./chart'
            } else {
                error("Deployment is allowed only during the change window (10 PM - 11 PM).")
            }
        }
    }
}

### In prometheus grafana lets say we have distributed monitoring lsts say resources are spread across diff region how we can build now.  
### In terraform dependecy error like linear & circular  
### What is lifecycle in TF  
Terraform lifecycle is a meta-argument that controls how resources are created, updated, or destroyed. The most commonly used lifecycle rules are create_before_destroy to minimize downtime during replacements, prevent_destroy to protect critical resources from accidental deletion, ignore_changes to ignore updates made outside Terraform, and replace_triggered_by to force resource replacement when dependent resources change. These settings help manage infrastructure safely in production.

# TechMahindra
---------------
### Suppose we have EKS cluster version X to upgrade upper version 3 node cluster while ensuring zero downtime? how did you do that?
### How did you upgrade the kubernetes cluster on zero downtime.
### Jenkins CI fails how to fix 
### Write a jenkins pipeline trigger from main branch Build & push to ACR before that in ci pipeline we need code scan onc done trigger to cd pipeline?  
### Azure pipeline taking 40 minutes in ci stage randomy fails in test stage how did you troubleshoot?  
### How did you integrate terraform to azure ci cd pipeline?  
### If someone manully create in cloud how to fix in term of terraform?
### You are in call getting crashloopback error.  
### Write a terraform file for first create one resource group on top of it create resource group, vnet , 2 subnet , private endpoint?  
### Difference bw nodeselector and taint and toleration?  
### Write a yaml for ingress pod name space should my namespace?  
### why we are using ingress into AKS?  
### what is pv and pvc ?  
### we have nodeselector in particular pod going to deploy new pod without objection on particular nodespace so it cant restrict that pod  
### i want build a new nodepool for our aks cluster i want version memory disk pace OS version network taint and toleration how to achive using yaml file.  
### In AKS we have 3-4 pipeline dev uat test n prod once development done go for next step prd time we have approval gateway and deploy to prod.  
### what is kubeconfig 
#### we have deploy 3 node cluster how many deploys 
master node build that 

# Github workflows & yaml exp migrating azure to githubaction flows , aks deployment .most deploy in aks.

# Photon
==========
### Questions: A jenkins pipeline takes 45 mins to build microservices Build how do you reduces it less tah 10 minutes without adding more agents.   
### Questions: Developers complains there is pipeline is getting failed no actionable log what do you change.
### Questions: A microservices in kubernetes become slow during peak hours no code chnage what do you investigate?  
### Questions: Elastic search cluster storage cost is expoding what is your plan?  
### Question: Kafka consumer log grows during spike how do you troubleshoot it.  
### Question: Production hotfix must go urgenty but multiple teams have ongoing change in developed branch?   
### Questions: CPU spike for a workload but no alert is triggered how do you fix monitoring?  
### Question: A service itermittently become unreachable how do you troubleshoot?   
### Question : JAVA 8 service must upgrade to JAVA17 how do you scale large migration.  
### Question: A critical incident happen at night you are esclation point what your action plan?  
### Question: How do you design a zero downtime multi region deployment?   
### Question: A deployment to production fail half way some pods are running older version some in new version what do you do?   
### Question: Jenkins pipeline is frequently fails due to intermitent network issue while pulling dependencies what your approach?   
### Question: Kafka topics retention eating up disk what your quick fix?    
### Question: your cluster root certificate expire in next 5 days what is your plan?   
### Question: your jenkins pipeline takes 20 mins before even starting unit test becasuse its download dependecies every time how do you fix it without modifying developer machine?  
### Question: Latency spike occured during high load metrics show cpu is fine but conetxt switching is high?  
### Question: A pod JVM service uses 80% cpu deposite load traffic what do you inspect?  
### QuestionA Sudden spike in 400 error occur what you check first as devops perspective?  
### Question You must provide a audit log for all infrastructure changes what solution do you implement 
### Question A micro services deployment causes API Gateway 502 error even through pods are healthy what your approach
-----------------------------
# AHEAD questions asked me for AWS cloud devops Engineer
---------------------------------------------------------------------
### Question: Recent infrastructure architecture in aws  >> see in aws file infra provisioning project.
In my recent project, we hosted a highly available microservices application on AWS using Amazon EKS. The infrastructure was deployed across multiple Availability Zones within a custom VPC. The VPC had public and private subnets. Public subnets hosted the Internet-facing Application Load Balancer and NAT Gateways, while private subnets hosted the EKS worker nodes and application workloads. We used Terraform to provision the infrastructure and Jenkins for CI. After the Docker image was built and scanned, it was pushed to Nexus/ECR. Argo CD handled continuous deployment to the EKS cluster using a GitOps approach.

# How do you approach sudden traffic spikes in production?
Step-by-Step Approach
Identify the cause – Genuine traffic or DDoS?
Check monitoring – CPU, memory, response time, error rates (CloudWatch, Prometheus, Grafana).
Scale the application – Auto Scaling Group (EC2) or HPA (Kubernetes).
Check dependencies – ALB, database, Redis/cache, queues.
Mitigate if needed – Use CloudFront, WAF, Shield, rate limiting.
Monitor and validate – Ensure the application remains healthy and stable.

### Question: Expalin vpc component and traffic flow how it happen  
VPC Components
VPC – Private network in AWS.
Public Subnet – Hosts internet-facing resources (ALB, Bastion Host, NAT Gateway).
Private Subnet – Hosts application servers, EKS worker nodes, RDS.
Internet Gateway (IGW) – Enables internet access for public subnets.
NAT Gateway – Allows private subnet resources to access the internet for outbound traffic only.
Route Table – Determines where traffic is routed.
Security Group (SG) – Stateful firewall attached to resources.
Network ACL (NACL) – Stateless firewall applied at the subnet level.
Elastic IP – Static public IP address.
VPC Endpoint – Private access to AWS services without using the internet.

### Question: How does NAT Gateway works internally  
A NAT Gateway enables instances in a private subnet to initiate outbound connections to the internet while preventing unsolicited inbound connections. Internally, it performs Source Network Address Translation (SNAT) by replacing the private IP address of the instance with its own Elastic IP address. It also maintains a connection tracking table so that when the response returns from the internet, it translates the destination back to the original private IP and forwards the traffic to the instance. 

### Question: diff b/w security group vs nacl  
A Security Group is a stateful firewall attached to an EC2 instance or network interface. It allows only permit rules, and return traffic is automatically allowed. A Network ACL is a stateless firewall applied at the subnet level. It supports both allow and deny rules, and both inbound and outbound traffic must be explicitly configured. In production, Security Groups are used to control access to individual resources, while NACLs provide an additional security layer for the entire subnet. 

### Question: How to create  highly availabe web application in aws  
### Question:  Diff b/w ALB VS NLB when do you choose 
# ALB vs NLB

| **ALB (Application Load Balancer)** | **NLB (Network Load Balancer)** |
|-------------------------------------|---------------------------------|
| Operates at **Layer 7 (HTTP/HTTPS)** | Operates at **Layer 4 (TCP/UDP/TLS)** |
| Routes based on **URL path and host name** | Routes based on **IP address and port** |
| Supports **path-based** and **host-based routing** | Does **not** support path-based routing |
| Supports **SSL termination** | Supports **TCP/TLS pass-through** and **TLS termination** |
| Best for **web applications** and **microservices** | Best for **high-performance TCP/UDP applications** |
| Slightly **higher latency** | **Very low latency** and **high throughput** | 

### Question: Explain me CI/CD pipeline which you have build in AWS  
### Question: EC2 based application is slow during high traffic cpu is normal response time is high how to troubleshoot.   
# EC2 Application Slow During High Traffic (CPU Normal, Response Time High)

## Troubleshooting Steps

### 1. Check ALB Metrics
- Request Count
- Target Response Time
- HTTP 5XX Errors
- Healthy/Unhealthy Targets

---

### 2. Check Application Logs
- Slow API responses
- Exceptions and errors
- Thread blocking / Thread pool exhaustion
- Application response time

---

### 3. Check Database
- Slow queries
- Database connections
- CPU utilization
- IOPS
- Read/Write latency

---

### 4. Check Cache
- Redis/Memcached health
- Cache hit ratio
- Cache latency

---

### 5. Check Network & External APIs
- API latency
- DNS resolution issues
- Network latency
- Third-party API response time

---

### 6. Check EC2 Instance
- Memory Usage
  ```bash
  free -h
  ```

- Disk I/O
  ```bash
  iostat
  ```

- Network Statistics
  ```bash
  sar -n DEV
  ```

- System Load
  ```bash
  top
  vmstat
  ```

---

## Common Root Causes
- Slow database queries
- Database connection pool exhausted
- External API latency
- High disk I/O
- Memory pressure or swapping
- Network latency
- ALB target response delay
- Application thread pool exhaustion
### Question: Production application behind ALB app LB return 502 error intermitently how to troubleshoot.
# Troubleshooting Steps

## 1. Check ALB Metrics (CloudWatch)
- HTTPCode_ELB_502_Count
- TargetResponseTime
- RequestCount
- HealthyHostCount
- UnHealthyHostCount

---

## 2. Check Target Group
- Verify target health
- Ensure targets are registered
- Check health check path
- Check health check port
- Review health check timeout and interval

```bash
aws elbv2 describe-target-health --target-group-arn <target-group-arn>
```

---

## 3. Check Application Logs
- Application exceptions
- Application crashes
- OutOfMemoryError
- Thread pool exhaustion
- Slow API responses

---

## 4. Check EC2 Instance
- CPU utilization
- Memory usage

```bash
free -h
```

- Disk usage

```bash
df -h
```

- Disk I/O

```bash
iostat
```

- System load

```bash
top
vmstat
```

---

## 5. Verify Application Port

Ensure the application is listening on the configured port.

```bash
ss -tulpn
```

or

```bash
netstat -tulpn
```

---

## 6. Check Security Groups & NACL

- ALB Security Group → EC2 Security Group
- Target port allowed
- NACL rules allow traffic

---

## 7. Check Network Connectivity

From EC2

```bash
curl http://localhost:8080/health
```

or

```bash
curl http://<private-ip>:8080/health
```

---

## 8. Check ALB Timeout

- Idle Timeout
- Application response timeout
- Backend timeout configuration

---

## 9. Check Deployment

- Recent deployment
- Rolling update failures
- New application version
- Container restart count (Kubernetes)

---

## Common Causes of 502

- Application process stopped
- Wrong target port
- Failed health checks
- Application timeout
- Application crash
- Thread pool exhausted
- Memory issue
- Security Group misconfiguration
- Target deregistered
### Question: Your AWS bill jump from 1k to 4k in a month how do you identify the root cause and prevent for next time recurrence.  
# AWS Bill Increased from $1K to $4K – How to Identify the Root Cause?

## Interview Answer (1 Minute)

> If my AWS bill suddenly increased from **$1K to $4K**, I would first identify **which AWS service caused the increase** using AWS Cost Explorer and the Cost & Usage Report. Then I would drill down by account, region, service, and resource tags to identify the exact resource responsible. After finding the root cause, I would take corrective actions such as terminating unused resources, rightsizing instances, enabling lifecycle policies, or optimizing storage. Finally, I would implement preventive measures like AWS Budgets, Cost Anomaly Detection, tagging policies, and regular cost reviews to avoid recurrence.

---

# Troubleshooting Steps

## 1. Check AWS Cost Explorer
- Compare current month vs previous month
- Identify which service increased the cost
- Filter by:
  - Service
  - Region
  - Account
  - Usage Type
  - Tags

---

## 2. Check Cost & Usage Report (CUR)
- Identify the exact resource
- EC2
- EBS
- S3
- NAT Gateway
- RDS
- Data Transfer
- Load Balancer

---

## 3. Identify Root Cause

Examples:
- New EC2 instances launched
- Auto Scaling misconfiguration
- Unused EBS volumes
- Large snapshots
- NAT Gateway data processing charges
- High internet data transfer
- S3 storage growth
- RDS storage increase
- Load balancer running continuously

---

## 4. Verify Recent Changes

- Terraform changes
- CloudFormation deployment
- Jenkins deployment
- Auto Scaling policies
- New application rollout

---

## 5. Take Corrective Actions

- Stop/Delete unused EC2 instances
- Delete unattached EBS volumes
- Remove old snapshots
- Enable S3 Lifecycle policies
- Resize EC2/RDS instances
- Optimize NAT Gateway usage
- Use VPC Endpoints for AWS services
- Purchase Savings Plans or Reserved Instances

---

# Prevent Future Recurrence

- Configure AWS Budgets
- Enable AWS Cost Anomaly Detection
- Apply mandatory resource tags
- Schedule automated cleanup of unused resources
- Review Cost Explorer weekly
- Configure CloudWatch billing alarms
- Enable Trusted Advisor cost recommendations

---

# Common AWS Services That Increase Cost

- EC2
- EBS
- RDS
- NAT Gateway
- Data Transfer
- S3
- Load Balancer
- CloudWatch Logs
- Elastic IPs
- EKS Worker Nodes

---

# Interview Answer (30 Seconds)

> I first use **AWS Cost Explorer** to identify which service caused the cost increase. Then I analyze the **Cost & Usage Report** to find the specific resource responsible. I verify recent infrastructure changes, optimize or remove unnecessary resources, and implement preventive controls such as **AWS Budgets, Cost Anomaly Detection, tagging policies, CloudWatch billing alarms, and regular cost reviews** to prevent similar issues in the future.
>

### Question: we have 50 developers need aws access no shared access allowed how to achive using tf  
I would automate access management using Terraform by creating IAM groups, users, and least-privilege policies. Permissions are assigned to groups, and developers are added to the appropriate group. In production, I prefer AWS IAM Identity Center integrated with an identity provider such as Microsoft Entra ID or Okta, allowing each developer to have an individual account with SSO, MFA, and role-based access without any shared credentials  

### Question: how do you enable zero downtime deployment
### Questions what sort of automation you have done.  
# What Sort of Automation Have You Done?

## Interview Answer (2 Minutes)

> In my recent projects, I automated infrastructure provisioning, application deployments, configuration management, monitoring, security scanning, and routine operational tasks. This reduced manual effort, improved deployment consistency, and minimized production errors.

---

## 1. Infrastructure Automation
- Provisioned VPC, EC2, ALB, Auto Scaling Groups, RDS, IAM, and EKS using Terraform.
- Reused Terraform modules across multiple environments (Dev, QA, UAT, Prod).

---

## 2. CI/CD Automation
- Built Jenkins pipelines triggered by GitHub webhooks.
- Automated:
  - Build
  - Unit Testing
  - SonarQube Scan
  - Trivy Scan
  - Docker Image Build
  - Push to ECR/Nexus
  - Deployment to EKS using Argo CD

---

## 3. Kubernetes Automation
- Automated application deployments using Helm.
- Configured HPA for auto scaling.
- Automated rolling updates and rollbacks.
- Managed Kubernetes manifests through GitOps.

---

## 4. Configuration Management
- Used Ansible to automate:
  - Package installation
  - User creation
  - Server hardening
  - Application deployment
  - Configuration updates

---

## 5. Monitoring Automation
- Configured CloudWatch alarms.
- Automated Prometheus monitoring.
- Created Grafana dashboards.
- Configured Slack/Email alerts.

---

## 6. Security Automation
- Automated vulnerability scanning using Trivy.
- Integrated SonarQube quality gates.
- Managed secrets using AWS Secrets Manager.
- Automated IAM role and policy provisioning.

---

## 7. Linux Automation
- Wrote Shell scripts for:
  - Log cleanup
  - Disk space management
  - Backup automation
  - Service health checks
  - Process monitoring

---

## 8. Python Automation
- Used Python (Boto3) to automate:
  - EC2 management
  - S3 bucket operations
  - Snapshot creation
  - Start/Stop EC2 instances
  - AWS resource reporting

---  
### Question: write ebs module and it should attach to ec2 
# Terraform EBS Module

## Project Structure

```text
terraform/
├── main.tf
├── variables.tf
├── outputs.tf
└── modules/
    └── ebs/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

---

## modules/ebs/variables.tf

```hcl
variable "availability_zone" {
  type = string
}

variable "size" {
  type = number
}

variable "volume_type" {
  type    = string
  default = "gp3"
}

variable "instance_id" {
  type = string
}

variable "device_name" {
  type    = string
  default = "/dev/sdf"
}

variable "tags" {
  type    = map(string)
  default = {}
}
```

---

## modules/ebs/main.tf

```hcl
resource "aws_ebs_volume" "this" {
  availability_zone = var.availability_zone
  size              = var.size
  volume_type       = var.volume_type

  tags = var.tags
}

resource "aws_volume_attachment" "this" {
  device_name = var.device_name
  volume_id   = aws_ebs_volume.this.id
  instance_id = var.instance_id

  force_detach = false
}
```

---

## modules/ebs/outputs.tf

```hcl
output "volume_id" {
  value = aws_ebs_volume.this.id
}
```
### Question: my statefile is locked you are trying to deploy you cant able to deploy what will you do?  
> If Terraform reports that the **state file is locked**, I first verify whether another Terraform operation is currently running, such as `terraform apply` or `terraform destroy`, because Terraform locks the state to prevent concurrent changes. If another deployment is in progress, I wait for it to complete. If the process has crashed or the lock is stale, I identify the lock holder and safely remove the lock using `terraform force-unlock`. Before unlocking, I ensure no one else is actively using the state to avoid state corruption.

---

# Troubleshooting Steps

## 1. Read the Error Message

Terraform usually displays:
- Lock ID
- Who acquired the lock
- Timestamp
- Backend information (S3, DynamoDB, Azure Blob, etc.)

---

## 2. Verify Another Deployment

Check:
- Jenkins pipeline
- GitHub Actions
- Azure DevOps pipeline
- Another engineer running Terraform locally

If yes, **wait for the deployment to complete**.

---

## 3. Check Backend Lock

### AWS (S3 + DynamoDB)

Verify the lock exists in the DynamoDB table.

---

## 4. If the Lock is Stale

If the previous Terraform process crashed:

```bash
terraform force-unlock <LOCK_ID>
```

Example:

```bash
terraform force-unlock 9f0e6f24-3f8d-4c6f-a7d1-123456789abc
```

---

## 5. Verify State

```bash
terraform plan
```

Ensure the state is consistent before applying changes.

---

## 6. Deploy Again

```bash
terraform apply
```

---

Get thses question from whatapp resources.
========================
1. Production AKS app went down at 2 AM. What is your immediate action plan?
 2. Pods are in CrashLoopBackOff after deployment. How will you troubleshoot?
 3. Deployment works in Dev but fails in Prod. What will you compare first?
 4. Terraform apply failed midway and some Azure resources are created. What next?
 5. Someone modified Azure resource manually which is managed by Terraform. How do you handle drift?
 6. AKS cannot pull image from ACR. How will you debug?
 7. Production pods CPU is 90 percent. What steps will you take?
 8. AKS node shows NotReady. What is your investigation approach?
 9. How will you design zero downtime deployment for critical application?
 10. Azure DevOps pipeline is successful but changes not reflecting in AKS. Where will you check?
 11. How will you structure Terraform for Dev, QA, and Prod environments?
 12. How do you secure secrets in Kubernetes for production workloads?
 13. Cluster autoscaler is not scaling nodes even when CPU is high. What will you verify?
 14. Terraform state file accidentally deleted. What are recovery steps?
 15. How will you configure private AKS cluster with restricted access?
 16. HPA configured but pods are not scaling. What could be wrong?
 17. Production deployment failed and business wants immediate rollback. What is your approach?
 18. How will you migrate manually created Azure infrastructure to Terraform?
 19. CI/CD pipeline taking 40 minutes. How will you optimize it?
 20. Multiple engineers working on same Terraform code. How do you prevent conflicts?
 21. How will you expose AKS app using Application Gateway instead of LoadBalancer?
 22. Ingress configured but traffic not reaching pods. What will you check?
 23. Node showing disk pressure error. How will you fix?
 24. How will you restrict production access only to specific IP addresses?
 25. Application needs persistent storage in AKS. What will you use and why?
 26. After new release, some users still see old version. What could be the issue?
 27. How will you implement Blue Green deployment in AKS?
 28. Security team says AKS cluster is publicly accessible. How will you secure it?
 29. How will you rotate secrets without downtime?
 30. There is complete production outage. How do you handle troubleshooting and stakeholder communication?




