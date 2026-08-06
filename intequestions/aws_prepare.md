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
Security Groups
Performance
