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

## Creating an EC2 instance with t2.large ubuntu 24  with 30 GB disk .

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

# Add ubuntu user to docker group
echo "👤 Adding 'ubuntu' user to docker group..."
sudo usermod -aG docker ubuntu
newgrp docker <<EONG
echo "🔄 Switched to docker group inside script"
EONG

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

## Run the Project locally

First we should have docker compose "docker compose -h " 
because by docker compose we can run multiple container at once.

build,  run , network , volume 

## now we are clonig project to our local instance.

```bash
 git clone https://github.com/iam-veeramalla/ultimate-devops-project-demo.git

```
cd ultimate >> ls >> we have docker compose file 
```bash
cd ultimate
ls 

It will running in background by using -d 

docker compose up -d  
```
## Now we have to go security group and add inbound rule 8080 so that we can access our portal.

## move to product-catalog in shell and the head to github under src >> product-catalog  README file 

First we have to export product catalog 

```bash
export PRODUCT_CATALOG_PORT=8080
go --version

sudo apt install golang-go

go build -o product-catalog >> its creating a build -o stands dest.

ls -ltr 

./product-catalog

```

## containerized of first micro-service -product catalog

```bash
cd product-catalog/
ls
rm Dockerfile

```

vi Dockerfile

```bash
FROM golang:1.22-apline AS builder

WORKDIR //user/src/app   ## directory for docker cmd execution.

RUN go mod download

RUN go build -o product-catalog ./

FROM alpine AS release ## here we use multistage docker file it will use above image with light weight

WORKDIR /usr/src/app/

COPY ./products/ ./products/

COPY --from=builder /usr/src/app/product-catalog/ ./

ENV PRODUCT_CATALOG_PORT 8088 
ENTRYPOINT [ "./product-catalog" ]

```
Now we have to build

```bash
docker build -t github acc name/product-catalog:v1 .

docker images |grep kamran

```
## Containerization of AD microservice.java

cd src   >> ls 

cd ad  && ls 

## java can build be build as maven or gradle we are using gradle 
As a Devops engineer we have to go through the documentation of each service in our case it is in github .
```bash 
java --version  

sudo apt update -y
sudo apt install -y openjdk-21-jdk-headless

```
lets follow the developer instructions.

```bash

./gradlew

if permission issue

chmod +x ./gradlew

```

## start Daemon , install dependencies, compile an app & Build APP & created build dir.

```bash

./gradlew installDist

ls -ltr ## to chcek its create a build dir

To check executable is created 

ls -ltr ./build/install/opentelemetry-demo-ad-/bin/Ad

ls -ltr ./build/install/opentelemetry-demo-ad-/bin/

export AD_PORT=9099

export FEATURE_FLAG_GRPC_SERVICE_ADDR=featureflagservice:50053

./build/install/opentelemetry-demo-ad/bin/Ad

## Containerization of Ad microservice part-2
```bash

cd /ultimeate-devops-project-dem/src/ad 

vi Dockerfile

FROM eclipse-temurin:21-jdk AS builder

WORKDIR /usr/src/app/    ## says this is directory where instrcution executed.

COPY gradlew* settings.gradle* build.gradle .
COPY ./gradle ./gradle

RUN chmod +x ./gradlew
RUN ./gradlew
RUN ./gradlew downloadRepos

COPY ..
COPY ./pb ./proto
RUN chmod +x ./gradlew
RUN ./gradlew installDist -PprotoSourceDir=./proto

#####################################################

FROM eclipse-termurin:21-jre 

WORKDIR /usr/src/app/

COPY --from=builder /usr/src/app/ ./

ENV AD_PORT 9099

ENTRYPOINT ["./build/install/opentelemetry-demo-ad/bin/Ad"]


docker build -t kamra/adservice:v1 .

docker run kamran/adservice:v1

```

## Contenarization of Recommendation microservice - python

```bash
cd /src/recommendation
rm -rf Dockerfile

vi Dockerfile

FROM python:3.12-slim-bookworm AS base

WORKDIR /usr/src/app

COPY requirements.txt ./

RUN pip install --upgrade pip
RUN pip install -r requirements.txt

ENTRY ["python", "recommendation_server.p"]

docker build -t kamran/recommendationservice:v1 .

docker run kamran/recommendationservice:v1

## Docker init- Simple wy of writing Dockerfile

```bash

cd src/shipping

docker init  y

Overwrite >> YES

Project USE >> Rust

Version >> 1.85.0

Target >> shipping

port >> 7077

ls -ltr

docker build .

We have to change Rust version = 1.76

docker build .

```

## Push the Container images to Registry 
 ```bash

docker login docker.io
docker login 

docker images | grep kamran

docker push docker.io/kamran/product-catalog:v2

docker push docker.io/kamran/adservice:v2

docker push docker.io/kamran/recommendationservice:v1

Now we will refresh in docker hub we can find all 3 images.

```

## How conatiner images are managed in single registry in relatime.

Devops Engineer is responsible for containerization of certain microservices.

## Understanding the docker compose file for the project.

```bash

cd ultimtae devops 

docker compose down

docker compose up -d ## running in BG

 take public ip of instance and 8080 use port 

 # Need for Container Orchestration:


 # Infrastructure as Code using Terrafom.


## state file management
 
 Once we create any resources in cloud state file has its meta to keep track of resource weather we update or delete any action we take state file has its informations.

 ## how do you manage state file?
 To manage state there are two ways.  

 1: backend ( remote)  
 2: state locking  

 Race condition occur if dev1 & dev2 trying to update in aws same config
 once its locked its says locked by dev1 once it release then dev can proceed.  
 
 ## S3 is used for Remote backend.
 ## Dynamodb is used for remote locking.

 ## Create S3 bucket & DynamoDB using Terraform.

 only four Things need to remember for writing terraform file.

 1: Provider > Its telling where to create resource aws/azure  
 2: Resource > Says which resource want to create eg: EC2,S3 bucket. 
 3: Varible block > basically pass varibales to actual terrafom file.  
 4: output.tf > what output you want to see if its successful.  


 ```bash

 provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "example" {
  bucket = "demo-terraform-eks-state-bucket"
  
  lifecycle {
    prevent_destroy = false
  }

}

resource "aws_dynamodb_table" "basic-dynamodb-table" {
  name           = "terraform-eks-state-locks"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "LockID"

  attribute {
    name = "LockID"
    type = "S"
  }
}
  
```





































































