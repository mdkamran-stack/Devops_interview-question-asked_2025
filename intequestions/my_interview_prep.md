# Interview Preparations:
## ci/cd can you explain ci/cd with diff stages along with tools used.
----------------------------------------------------------------------
In my recent project we implemeted our ci/cd pipeline in AzureDevOps For asp.net & java based application hosted on Azure App Service using deployment slots for Zero-downtime release.

FOR CI, we used Azure Repos for version control, Any commit or PR merge into the **main** branch triggered the build pipeline. This pipeline restored dependencies, ran unit test, performed static code analysis via sonarqube and generated an artifact package The artifact was restored in AzureDevOps artifact feed for versioned deployment.

For CD, We used AzurePipeline to deploy first to a staging slot of the azure App service, once deployed we ran smoke test against the staging slot to validate the release. if everything passed, we used automated slot swap to promote staging to production wihout downtime 

We integrated approval gates in Azure DevOps Env, so production deployment required sign-off from the product owner it cant deployed until unless approval not granted.
if needed we could instantly roll back by swapping slot back.

###  Tools & Services used:  
. Azure Repos for source code.  
. Azure Pipeline (yaml) for build & release automation.  
. SonarQube for static code analysis.  
. AzureArtifact for artifact storage and package management.  
. Terrfaorm for infra as Code.  
. Azure App Service and AKS for hosting application.  

Stages in in CI/CD Pipeline.

| Build | Dev | QA | Stage | Prod |
|-------|-----|----|-------|------|
| Lower Env | → | → | Upper ENV | → |

## Question: What is Azure App service.  

Azure App Service is a fully managed PaaS that lets you build, deploy, and scale web apps and APIs quickly without managing infrastructure, with built-in CI/CD, scaling, and security.

==============================starts NEW=========================================

## what is multi layer docker file , what is diff b/w first image & 2nd image.

In the first image usually we build an application that would take a bit of resources. In second one artifcats would be ready just ha to run so only runtime would be there , so just an exmaple if the java application first one we have to build an application we have maven related tools to build and package it, but whereas on the second one we have JVM availabe and JAR fies it has to run. so it would be light weight in that sense the no.of tools on first one may be higher and 
on second one it would be very light weight.

## How do you connect AWS Cloud with On-Premises data center?”

Short Interview-Style Answer

"To ensure connectivity between AWS and on-premises, I’d typically use AWS Site-to-Site VPN for quick and secure encrypted tunnels, or AWS Direct Connect for high bandwidth and low latency. For larger environments with multiple VPCs and networks, I’d use AWS Transit Gateway as a central hub. This ensures secure, reliable, and scalable hybrid connectivity.

⚡ Pro Tip: If they press you further → say VPN is internet-based (cheaper, quick), Direct Connect is private (faster, more reliable), and Transit Gateway helps scale across many VPCs + on-prem networks.

## CI/CD explanations on based-on Jenkins (CI) & Argocd (CD)

For JAVA application.

We have git repository  where we have application source code As soon as developer raise a pull request to this git repository we have configured webhooks using the webhooks we trigger the jenkins pipeline what we have done we have used declarative jenkins pipeline coz declarative pipelines are easy pipelines to write & collaborate as part of declarative jenkins pipeline we run multiple stages some of the stages are the first stage would be "build" stage using build stage using maven as build language what we do we build an application and if the build stage is successful or the application is build successfully where we also executes some sort of unit tests, the next stage would perform some static code analysis using static code analysis we will verify that this applicaion is not exposed to any static code and after that we have some SAST & DAST tools where we verify the application security if the new change that the developer is writing introduces any security vulnerability if any of this things fails the we would send email notifications that we have configured if all stages successfully done then we would go forward we would create a docker image so we are running simple shell target to create the docker image out of the docker file which we have stored in git repository itself & as soon as docker image is created again using shell cmd we push this docker image to ECR, this our CI process.

for CD we use ArgoCD once the docker image is pushed to the elastic conatiner reg.
We have a K8s clsuter inside k8s clsuter we have deploy two coninuos deivery tool.
1. image updater called IA argo image updater , other tool we have ARGOCD.
Both thing are k8s controller that we have deployed in k8s cluster what they do te first tool argo image updater it would continuously monitor the image registry as soon as image is created it will pick the new image and update the new image in another git repository that we have & this repository is purely image manifest.
that is our HELM chart , as soon as this github repository is updated with new image then that other k8s controller we have i.e argocd it takes new image and deploy on the k8s clsuter, so this is our cd process ,this is how we setup ci/cd in our organization.

# Jenkins Pipeline for Java based application using Maven, SonarQube, Argo CD, Helm and Kubernetes

![Screenshot 2023-03-28 at 9 38 09 PM](https://user-images.githubusercontent.com/43399466/228301952-abc02ca2-9942-4a67-8293-f76647b6f9d8.png)
