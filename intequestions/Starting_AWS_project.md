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

# Docker installations & Kubectl and terraform sciprt in one short

```bash
#!/bin/bash
set -e

echo "🚀 Starting setup on Ubuntu 24.04 LTS..."

#######################################
# 1. Remove old installations
#######################################
echo "🧹 Removing old Docker, Terraform, and kubectl..."
sudo apt-get remove -y docker docker.io docker-doc docker-compose podman-docker containerd runc || true
sudo apt-get purge -y terraform kubectl || true
sudo snap remove kubectl || true
sudo rm -f /snap/bin/kubectl
hash -r || true

#######################################
# 2. Install Docker
#######################################
echo "📦 Installing Docker..."
sudo apt-get update -y
sudo apt-get install -y ca-certificates curl gnupg lsb-release

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update -y
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

#######################################
# 3. Install kubectl (v1.30 stable)
#######################################
echo "☸️ Installing kubectl..."
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | \
    sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
  https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /" | \
  sudo tee /etc/apt/sources.list.d/kubernetes.list > /dev/null

sudo apt-get update -y
sudo apt-get install -y kubectl

# Ensure correct kubectl path
sudo rm -f /snap/bin/kubectl
hash -r

#######################################
# 4. Install Terraform
#######################################
echo "🌍 Installing Terraform..."
curl -fsSL https://apt.releases.hashicorp.com/gpg | \
    sudo gpg --dearmor -o /etc/apt/keyrings/hashicorp-archive-keyring.gpg

echo "deb [signed-by=/etc/apt/keyrings/hashicorp-archive-keyring.gpg] \
  https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
  sudo tee /etc/apt/sources.list.d/hashicorp.list > /dev/null

sudo apt-get update -y
sudo apt-get install -y terraform

#######################################
# 5. Verify versions
#######################################
echo "✅ Versions installed:"
docker --version
kubectl version --client
terraform version

```
