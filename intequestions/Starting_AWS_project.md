# Project Starts

## IAM User

Auth and authorization in AWS

AUTH > user/group
Authrization > Roles/policies (permission)(read,write,delete,update) Goes to user.
in organization we have created user with auto generated password once user login its asked to change password best practice it is.

While creating user 
1. asked Add a user to group  
2. Copy permission  
3. Attach policies directly we can assign here admin or as per requirement

## Creating an EC2 instance with t2.large ubuntu 24  

login to ec2 instance

## Docker installations:

Install Docker on Ubuntu 24.04 (EC2)
Step 1: Update system  
sudo apt update && sudo apt upgrade -y  

Step 2: Install required packages  
sudo apt install -y ca-certificates curl gnupg lsb-release  

Step 3: Add Docker’s official GPG key  
sudo install -m 0755 -d /etc/apt/keyrings  
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg  

sudo chmod a+r /etc/apt/keyrings/docker.gpg  

Step 4: Setup Docker repository  
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null  

Step 5: Install Docker Engine  
sudo apt update  
sudo apt install -y docker-ce docker-ce-cli containerd.io     docker-buildx-plugin docker-compose-plugin  

Step 6: Start and enable Docker  
sudo systemctl start docker  
sudo systemctl enable docker  

Step 7: Verify installation  
docker --version  

## Install kubectl on Ubuntu 24.04  
Step 1: Update packages  
sudo apt update  

Step 2: Download the latest stable release of kubectl
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"  

Step 3: Make it executable & move to PATH  
chmod +x kubectl  
sudo mv kubectl /usr/local/bin/  

Step 4: Verify installation  
kubectl version --client  

You should see something like:  

Client Version: v1.30.x  

## Install Terraform on Ubuntu 24.04 

Step 1: Update and install required packages    
sudo apt update && sudo apt install -y gnupg software-properties-common curl  

Step 2: Add HashiCorp GPG key  
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg  

Step 3: Add HashiCorp repository  
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list  

Step 4: Install Terraform  
sudo apt update  
sudo apt install -y terraform   

Step 5: Verify installation  
terraform -version  
