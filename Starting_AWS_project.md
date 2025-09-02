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

go build -o product-catalog >> its creating a build & download dependencies -o stands dest.

ls -ltr 

./product-catalog

```
## As a first step we create docker file
## 2: build docker image
## 3: we will containerize it.

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

ENV PRODUCT_CATALOG_PORT=8088 
ENTRYPOINT [ "./product-catalog" ]

```
Now we have to build

```bash
docker build -t github acc kamran112/product-catalog:v1 .
docker images |grep kamran
docker run kamran112/product-catalog:v1

We follows developers documents reg ENV variable & version and follows the documents

start the daemon 
install the dependecies
complitaion 
Build dir where executable & jar file put 


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


docker build -t kamran112/adservice:v1 .

docker run kamran/adservice:v1

We have containerized java microservice and GO microservice as well.


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
 3: Varible file > basically pass varibales to actual terrafom file.  
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
## In organization if anyone want to provison any resources first we will check respective module if its there then we will invoke the module from main.tf.

## VPC and EKS Cluster part-1

vpc >> should associate private sbnet should be connected with NAT Gateway attach   

for public subnet should be connected to  IGW to access from internet 

To assicate public subnet we need "ROute table" will be associated with NAT Gateway as well as IGW and then associate the private n public subnet with route table.

step 1: create private subnet 
step2: create NAT gateway
step3: Route table will be connected to NAT gateway then both subnet & Route table will be associte.



These concept in aws Vermalla video refer

## if we want to make reusable then we have use variable.  
modules are reusable  thats why in module they invoke and ececuted in their code,the module invoked by main.tf if someone in us they will use, 
we start writing code by resources block.


# VPC & EKS cluster part2:

1: i am role >> cluster
2: i am role >> Node

## This is for k8s controlplane component.  
1: i am cluster
2: policy attached the policy with IAM role
3: eks cluster which will complete the control plane.  
## This is for k8s data plane component.
4: i am role for  node
5: create a policy attach to i am role
6: Node gorup attach it to EKS cluster


# In interview they ask where is your module stored?

As devops engineer we have a centralize location within organization where we store all the module like EKS , VPC , we souce invoke that module using this particular line

Eg: moulde "vpc" {
  source = "./modules/vpc"
}

We ave to sya that what is source of the module where to invoke module.
and what varibales we have to pass to the module.

# Deploying project to Kubernetes.

## How to connect to 1 or many k8s cluster from command line.

```bash

sudo apt install unzip -y 

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update

aws eks update-kubeconfig --region us-west-2 --name my-eks-cluster  

kubectl config view >> to list the cluster
kubectl config current-context >> To switch to desired cluster
kubectl get nodes  >> Get nodes i am connect to EKS cluster
aws eks update-kubeconfig --region us-west-2 --name my-eks-cluster >> To add cluster in qube config

kubectl config view >> see the EKS cluster.

kubectl config current-context >> show my current context

kubectl sa >> service account
kubectl get sa -n kube-system

```

## Kubernetes service account an its importance in rela time.

```bash
kubectl get sa
kubectl get sa -n kube-system

```
## In Interview may ask Service Account.

service acc of k8s Should be assigned with cluster role via binding resource.

Basically we will create a cluster role and defining permission on it then using role binding or clsuter roel binding to service account to role.

## Kubernetes Deployment

deployment is resposible for Healing and scaling in k8s.

## Kubernetes services. 

Service is responsible for service discovery & its reconize using lables & selectors.

## Deploying the project on kubernetes PART-1

##  Interview Q: How k8s service resource is define.

1: apiversion
2: kind: deployment 
3: metadata
4:name: name of deployment
5: labels >> which is only for identifying projects or the deployment during troubleshooting. ("This is not used for service discovery PART")
## IMP:deploymnet of our labels is not used for service discovery.
5: Lables of pod which play a critical role in service discovery.
The deployment of labels are not important for service discovery.
6: specs create a repicas as per defined for autoscaling and auto healing.  
7: template we define meta data for service identification.  
8: specs we provide service acc name where we have conatiner related info.

Below are constant in service.yml
ports might change specs might change sevice may change but fields are same

Specs
service acc name
image:
ports
env  are provided by developer
volume

## 3 types of service 
1 Nodeport >> Network within VPC 
2 cluster IP >> Only within cluster access.
3 Loadbalancer >> API talk to ccm anyboday can talk to it.

# Deploying the project on kubernetes PART-3

```bash
kubectl config current-context
kubectl get all
cd kubernetes
ls -ltr  
kubectl apply -f serviceaccount.yaml 
kubectl get sa    
ls
kubectl apply -f ad/*

## we have some mega yaml file so we can deploy at once.

kubectl apply -f complete-deploy.yaml

kubectl get pods

kubectl get svc

 kubectl get svc |grep frontendproxy
 kubectl edit svc opentelemetry-demo-frontendproxy
In last we have to change the cluster ip to LoadBalancer so we will access the application. (its is not a good approcah)

 kubectl get svc opentelemetry-demo-frontendproxy

```

kubectl edit svc svc name  ## in the last change cluster ip to LoadBalancer wait 5 mins for reflect.

we deploy kubernetes cluster using LoadBalancer service type

## Interview Question what is different b/w loadbalancer service tyoe and Ingress

If it is http & i want to make it HTTPS then we have to add certificate manually so there is no keeping track of it so its lack of declarative approach.
f5 LB Ngix no one is supporting k8S.

IT is cost in-effective approach for each n evry service we have to create LB 

CCM is only create ALB load balancer lack of felxibility.

How abt doing on cluster that doesnt have CCM component.

Ingress is good approach it can be define declarative LB can be updated and modified.

Cost-effective One LB can send route req to 100s of microservice typically hostbased or pathbase routing it is also very cost effective.

## anyotherways to advantage of using service of LB.
only one advantage very easy to configure goto service & change type to service type LB, Helm chart all can be reduced.


LB >> API >> CCM >> AWS >> LB & lb connect to LB
Last LB attached to service LB anyone can access from external.
ccm cloud control manager

1> Configuration of LB is not declarative (notthing of that supported by k8s service type)
2> it is also not cost effective 

3> ALB > You will be tied to the Alb load balancer itself. (lack of flexibility)

4> CCM how about doing it on a cluster that does not have Cloud control manager or CM component load balancer? Service type will not work.

## Interview Ques: When do you create Inress
Only service that needs external access is fronted proxy will create ingress resource Everythin else we dont need ingress resouce we only create 
service & deployment for frontend For Frontend Proxy we create Service deployment and Ingress as well.

## Ingress > So first advantage of using ingress it can be defined in declarative approach.

Because of that, and because of labels and annotations on your ingress, the load balancer can be updated

and modified, that is, declaratively using a YAML file.

Second, it is cost effective because even if you have ten microservices or 100 microservices using

Path and Host, you can just create one load balancer and 100 target groups.

So that just one load balancer will route request to ten or 20 or 100 microservices, typically using.  

service type LB is very easy to configure.

# Deploy the ALB Ingress controller

Interview Ques: How can a pod within eks cluster performed activity with rep to resource outside EKS cluster?

Ans: Every pod has a service account what we will do we will create IAM role and assign policy 

## Ingress and Ingress controller

Ingress >> Ingress controller >> LB

If we need nginx then we need to use nginx ingress contoller

if we have to use F5 load balancer then we have to use F5 load balancer.

Now we will proceed how to create ALB controller & by suing ingress how to access project.

## Deploy the ALB ingress controller.

```
eksctl version  

Installation of eksctl
------------------
curl -sSLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz"
  
  tar -xvzf eksctl_$(uname -s)_amd64.tar.gz

sudo mv eksctl /usr/local/bin

eksctl version

kubectl config current-context >> To check i am connectd to cluster

kubectl config current-context  
```
## Interview QUES: How can a pod within a EKS cluster perform an activity with resp to resource outside of EKS cluster?

Every pod has a service account We will create IAM role & assign policy with reqd permission to IAM role as an step we will connect service ACC with IAM role through
IAM OIDC provider.

```bash
Setup ALB add on.
export cluster_name=my-eks-cluster

oidc_id=$(aws eks describe-cluster --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5)  

echo $oidc_id  
```

Now we will associtae iam oidc to cluster

```bash
eksctl utils associate-iam-oidc-provider --cluster $cluster_name --approve 
```
Download IAM policy

```
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json
``` 

Create IAM Policy

```
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json
```
Create IAM Role

```
eksctl create iamserviceaccount \
  --cluster=my-eks-cluster \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::<your-aws-account-id>:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve
```
  eksctl create iamserviceaccount \
  --cluster=my-eks-cluster \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::<your-aws-account-id>:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve --override-existing-serviceaccounts
```

## Deploy ALB controller

Add helm repo

```
helm repo add eks https://aws.github.io/eks-charts
```

## Helm installation:
```
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

Add helm repo

```
helm repo add eks https://aws.github.io/eks-charts
```

Update the repo

```
helm repo update eks
```

Install

```
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=my-eks-cluster --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller --set region=us-west-2 --set vpcId=291515988309
```
```bash
command to check logs in kube-system
kubectl logs aws-load-balancer-controller-5476bcb474-c2h2w -n kube-system
```
Verify that the deployments are running.

```
kubectl get deployment -n kube-system

```

to check the log specific pod

kubectl logs pod id

## Setup ingress resources & access project.

cd ultimate project
cd frontendproxy/

Inress.yaml for fronted proxy 

vi ingress.yaml
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: frontend-proxy
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
spec:
  ingressClassName: alb
  rules:
    - host: example.com
      http:
        paths:
          - path: "/"
            pathType: Prefix
            backend:
              service:
                name: opentelemetry-demo-frontendproxy
                port:
                  number: 8080
```
```
kubectl apply -f ingress.yaml

if somehow address not showing then we have to troubleshoot.

kubectl get ing

kubectl get pods -n kube-system

kubectl logs Loadbalanser name -n kube-system



vi /etc/hosts

## Custom domain configuration.

Login Godaddy get the doamin kamran.shop

## Creating hosted zone in Route53

Goto Route53 >> select Create hosted Zones.
Domain name: whatever we have set for & purchase from Godaddy
desc & select Public hosted zone done.
Click on Create Record
Record name  www
Route traffic to " Alias to application and classic load balancer
Select region:
Select loadbalancer.
Routing policy 
Simple routing  create record.

Now we have to copy Nameserver record and paste it to Godaddy so it will route the request to LB.

Once copy done goto Godaddy DNS management.
Under name server change the Nameserve I will use my nameserver.put all 4 nameserver.

It will take 2-48 hours to reflect.

we can check by whatsmydns.net

# CI/CD

CI GItHub action

On Every pull request happen Once code checkout then automatically CI ran ci pipeline ran Unit test and then Static code
analysis happen and ran build &create docker image & scan the docker image push newly created image and update the k8s manifeat.
once CI passed developer create a PR 
CD : Using Argo CD & deploy new image to k8S cluster.

CI operate build integration
CD operates Delivery

## What is Github Action 
Github Action is CI orchestrator that is provided by Github.

If we have source code in github repo we have to create a folder .github & workflows & within .github we need to place yaml file
which will instruct Github action what needs to be run

This yaml file provide all the instructions what needs to be done, Github provide Action this action provide module, like
action provide cloning a repository no need to git use git clone ,
simillary for docker login we no need to write docker login or push cmd simillary for java , Go , all thie we write in yaml
file is the github action that why it is called github action
CI,coz github provide plugin/module which wil place yaml file within the workflow we will write action


CI file: Always start as below

Name: product
on : pull/push
jobs: build 
runs on : ubnut-latest
steps checkout
- build
- unit test


#
# Gitub Action Implementing CI for a microservice

# CI for product catalog Service

name: product-catalog-ci

on: 
    pull_request:
        branches:
        - main

jobs:
    build:
        runs-on: ubuntu-latest ## we can put self-hosted runner as well.

        steps:
        - name: checkout code
          uses: actions/checkout@v4

        - name: Setup Go 1.22
          uses: actions/setup-go@v2
          with:
            go-version: 1.22
        
        - name: Build
          run: |
            cd src/product-catalog
            go mod download
            go build -o product-catalog-service main.go

        - name: unit tests
          run: |
            cd src/product-catalog
            go test ./...
    
    code-quality:
        runs-on: ubuntu-latest

        steps:
        - name: checkout code
          uses: actions/checkout@v4
        
        - name: Setup Go 1.22
          uses: actions/setup-go@v2
          with:
           go-version: 1.22
        
        - name: Run golangci-lint
          uses: golangci/golangci-lint-action@v6
          with:
            version: v1.55.2
            run: golangci-lint run
            working-directory: src/product-catalog

    docker:
        runs-on: ubuntu-latest

        needs: build

        steps:
        - name: checkout code
          uses: actions/checkout@v4

        - name: Install Docker
          uses: docker/setup-buildx-action@v1
        
        - name: Login to Docker
          uses: docker/login-action@v3
          with:
            username: ${{ secrets.DOCKER_USERNAME }} ## for that we need secrets in github proj sett secret & action Repository n secret.

            password: ${{ secrets.DOCKER_TOKEN }}
            Under Docker hub acc settings personel access token & place that on Action secret value under github. with name Docker_Token

        - name: Docker Push
          uses: docker/build-push-action@v6
          with:
            context: src/product-catalog
            file: src/product-catalog/Dockerfile
            push: true
            tags: ${{ secrets.DOCKER_USERNAME }}/product-catalog:${{github.run_id}}

    
    updatek8s:
        runs-on: ubuntu-latest

        needs: docker  ## This job is dependent on above job 

        steps:
        - name: checkout code
          uses: actions/checkout@v4
          with:
            token: ${{ secrets.GITHUB_TOKEN }} ## grant the access my personel access token. goto user sett dev sett personal access token classic new token.

        - name: Update tag in kubernetes deployment manifest
          run: | 
               sed -i "s|image: .*|image: ${{ secrets.DOCKER_USERNAME }}/product-catalog:${{github.run_id}}|" kubernetes/productcatalog/deploy.yaml
        
        - name: Commit and push changes
          run: |
            git config --global user.email "abhishek@gmail.com"
            git config --global user.name "Abhishek Veeramalla"
            git add kubernetes/productcatalog/deploy.yaml
            git commit -m "[CI]: Update product catalog image tag"
            git push origin HEAD:main -f
            


## What kind of static code you dela with .

we deal with developers not using functions created a function but not invoking function or using deprecated modules so static check failed.


# CD Intro GitOps using argo CD

-------------------------

In CI part we have seen 

GitHub Action  we have multiple stages for continuous integartion
----------------------------------------------------------------
we see Build >> Unit test >> docker image creation & push >> Finally updates k8s manifest .

# CD picks up this k8s manifest updated verison & deploy on k8s cluster.

CD tool we used GitOps is an approach where k8s manifest which got updated by CI stage stored in version control system, CD Argo CD read this changes and deploy them to target platform k8s.

1: Automatic deployment:

Gitops pick changes from version control system and deploy into target system.

2: Reconciliation: if someone change in cluster from v2 to v1 Argocd continously monitoring and it will change to previous state, because in Gitops version contolr is source of truth
if ci updates a particular new version from v1 to v2 only then Argo CD pickup this version and deploy to k8s, state is maintain by Argo deploy in existing version in vcs.

## CD PART 

1 install ARGOCD 

2 kubectl get pods -n argocd

kubectl get svc -n argocd  ## argocd server is user interface of argocd

kubectl edit svc argocd-server -n argocd ## service type LoadBalancer

kubectl get secret -n argocd  ## wll see argocd secret 

kubectl edit secret argocd-initial-admin-secret -n argocd  ## copy passwd

it is base 64 encode we have to decode it

echo passwd == |base64 --decode  ## now we can login to argocd

Now we'll configuring argocd

create application 

Sync policy should be Automatic so it will automatically detect ny changed in git repo & deploy to clsuter by default it take 180 sec to pickup new image & deploy to cluster.
argocd is capavle to deploy HELM chart deploy plane manifest

Put the path where reside our deploy.yaml as well service.yaml 

Set the cluster as per desired deployment.  & apply now it will picking up deployment.




# About Project in Interview .

Hi my name kamran in my current org , where organization has E-commerce application 
and this E-commerece application is multi micro service architecture because there are more tha 100 micro services there are multiple
 development teams is responsible for certain microservices probaly 3-4 microservices, as a devops engineer my role is to work closely
with two development teams so my project is basically to work with both of thise devlopment team and implement DevOps or constantly
improve the devops practices for both devlopment teams and as a part of that i work on CI,CD of taht project.
I work on writing Terraform infra as code for them and work on k8s implementation.
Constantly improving the existing scripts.
So this is my project where I work for the development teams.
i work with payments and cart devlopment teams
I am responsible for DevOps implementation of those microservices 

E-commerce site they sells pharma.

# Day to day activities:

 I work for two development

teams where I closely attend their meetings and I work with them in agile methodology, where these development teams plan activities during the sprints,
or they plan activities for the entire quarter,and I closely involved with them during that meetings, so that I understand the roadmap of those development
teams for the coming quarter.
And with that, I can also estimate the amount of work that is going to come to me, and I would plan my activities accordingly.
some time team requests me to create some infrastructure because I am the one who developed and manage infrastructure as code for those development teams.
One of the recent activity that I worked on was to set up ECS cluster within VPC, and I set that up in the module structure so that going ahead, either
other DevOps teams or someone who is joining my team or someone who is going to work along with me, can also use those Terraform modules.
Along with that, I get a lot of requests related to Kubernetes deployments.
---------------------------------
I have requests to set up GitOps practices for the development teams,
related to AWS.
And there are some frequent activities like git management where we have multiple git repositories. So I set up webhooks for that.
I manage the git repositories with respect to branching.
I make sure the branching strategy is up to date.
So these are some of the day to day activities that I work on.
On top of this, 
You can say I help the developers to containerize their microservices.
Some of the developers are very new.
There are junior developers, so I also help them with writing Docker files.
If you want to add one more point, which puts a lot of weightage, talk about documentation.
So you can say as a DevOps engineer, I also document the best practices, for example, how to containerize
with the best practices like Distroless images.
And I share this documentation across the teams so that any team which does not have the DevOps engineer they can follow the documents.


# What challenges that you have faced in your day to day life.

When I joined this organization, the development teams were creating infrastructure changes manually. Some people were doing it through Terraform CLI.

Fortunately, there was one particular microservice for which they started creating infrastructure using Terraform.

So what I have done, I have taken up that microservice where Infrastructure was implemented, but it was lacking remote back end, and it
 was also lacking the state locking implementation, because of which the developers were confused.

Sometimes the Terraform state was not updated because they lacked the knowledge of state locking.

They were facing issues while running Terraform.

So again, I did a proof of concept to show how to set up remote backend and state locking, and eventually
I transitioned the entire infrastructure, some of which was in AWS CLI, some of which was in AWS CloudFormation templates to terraform.

Now, all of the projects or all of the microservices infrastructure is created through Terraform.
=======================================
And right now I am working on modularization of Terraform because the infrastructure is huge.

I have already implemented 80% of modularization where VPC module, EC module, all of this is implemented.


# Issues faced during LB INgress provision same thing we can frame during interview

“One of the most challenging issues I recently resolved involved provisioning an AWS Application Load Balancer through the AWS Load Balancer Controller in EKS.
 Everything looked correct — IAM roles, subnet tags, controller deployment — but the ALB simply wouldn’t appear. I dug into the controller logs and found repeated
 errors about subnet discovery and security group authorization. The key error was: ‘InvalidGroup.NotFound: You have specified two resources that belong to different networks.’
That told me the controller was trying to authorize traffic between security groups in different VPCs — which AWS doesn’t allow.

I validated this by inspecting the VPC IDs of both security groups, and sure enough, they were mismatched. To fix it, I created a new security group in the correct VPC, opened port 80 for public access, and explicitly assigned it to the Ingress using annotations. I also ensured the controller was using IRSA for IAM authentication, patched the ServiceAccount, and restarted the controller. Once everything aligned — VPC, SG, IAM, and annotations — the ALB provisioned successfully and traffic flowed as expected.

This experience reinforced how critical it is to trace cloud resource relationships across layers — IAM, networking, and Kubernetes manifests — and how controller logs can be your best friend. It also showed my ability to stay focused through multi-hour debugging and deliver a clean, production-ready soluti

## steps followed

🛠️ How You Fixed It
✅ 1. Created a New Security Group in the Correct VPC
You created sg-00143fa7ec576ee35 in vpc-0bbe799d063d0395f, which is the VPC your EKS cluster and subnets live in.

✅ 2. Opened Port 80 for Public Access
You authorized inbound traffic on port 80 to allow ALB access:

bash
aws ec2 authorize-security-group-ingress \
  --group-id sg-00143fa7ec576ee35 \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0
✅ 3. Explicitly Assigned the Correct SG to Your Ingress
You added this annotation to your Ingress manifest:

yaml
alb.ingress.kubernetes.io/security-groups: sg-00143fa7ec576ee35
This forced the controller to use the correct SG from the correct VPC.

✅ 4. Restarted the Controller and Monitored Logs
You restarted the controller to pick up the changes:

bash
kubectl rollout restart deployment aws-load-balancer-controller -n kube-system
Then monitored logs to confirm successful ALB provisioning.

✅ Final Result
Your Ingress now shows a valid ALB DNS name:

Code
ADDRESS: k8s-default-frontend-xxxxxxxxxx.us-west-2.elb.amazonaws.com
Which means:

Subnet discovery succeeded

IAM permissions via IRSA worked

Security group configuration was resolved

ALB is live and routing traffic
