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

# VPC & EKS cluster part2:

1: i am role >> cluster
2: i am role >> Node

1: i am cluster
2: policy
3: eks cluster
4: i am role node
5: policy attach to i am role
6: Node gorup attach it to EKS cluster


# In interview they ask where is your module stored?

As devops engineer we have a centralize location within organization where we store all the module like EKS , VPC , we souce that module to particaular line

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

kubectl config view
kubectl config current-context
kubectl get nodes

```

## Kubernetes service account an its importance in rela time.

```bash
kubectl get sa
kubectl get sa -n kube-system

```
## In Interview may ask Service Account.

service acc of k8s Should be assigned with cluster role via binding resource.

Basically we will create a cluster role and defining permission on it then using binding to service account to role.

## Kubernetes Deployment

deployment is resposible for Healing and scaling.  

## Kubernetes services. 

Service is responsible for service discovery  

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
```

kubectl edit svc svc name  ## in the last change cluster ip to LoadBalancer wait 5 mins for reflect.

we deploy kubernetes cluster using LoadBalancer service type

LB >> API >> CCM >> AWS >> LB & lb connect to LB
Last LB attached to service LB anyone can access from external.
ccm cloud control manager

1> Configuration of LB is not declarative (notthing of that supported by k8s service type)
2> it is also not cost effective 

3> ALB > You will be tied to the Alb load balancer itself. (lack of flexibility)

4> CCM how about doing it on a cluster that does not have Cloud control manager or CM component load balancer? Service type will not work.

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
kubectl version  
kubectl config current-context  
```
Interview QUES: How can a pod within a EKS cluster perform an activity with resp to resource outside of kubernetes cluster?

Every pod has a service account We will create IAM role & assign policy with reqd permission to IAM role as an step we will connect service ACC with IAM role through
IAM OIDC provider.
```
export cluster_name=my-eks-cluster
oidc_id=$(aws eks describe-cluster --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5)  
echo $oidc_id  
```

Now we will associtae iam oidc to cluster
```
eksctl utils associates-iam-oidc-provider --cluster $cluster_name --approve 
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
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \            
  -n kube-system \
  --set clusterName=my-eks-cluster
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=  ## provide region
  --set vpcId= ## undere eks cluster go to VPC Ntworking info & copy.
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

kubectl get ing

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

CI operate build integration
CD operates Delivery

## Github Action is CI orchestrator that is provided by Github.

If we have source code in github repo we have to create a folder .github & workflows
& within .github we need to place yaml file which will instruct Github action what needs to be run

This yaml file provide all the instructions what needs to be done, Github provide Action this action provide module, like action provide cloning a repository no need to git use git clone , simillary for docker login we no need to write docker login
or push cmd simillary for java , Go , all thie we write in yaml file is the github action that why it is called github action CI,coz github provide plugin/module which wil place yaml file within the workflow we will write action


CI file:

Name: product
on : pull/push
jobs: build 
runs: ubnut-latest
steps checkout
build 

























 






