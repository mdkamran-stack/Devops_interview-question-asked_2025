## About Project in Interview . Roels & responsiblities.

Hi my name kamran in my current org concentrix and client were regenron where organization has E-commerce application 
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

## Day to day activities:

As a DevOps Engineer in an Agile environment, I start by checking monitoring dashboards and CI/CD pipelines, then join daily stand-ups to align with sprint goals. During the day, I focus on backlog items such as infrastructure automation, deployments, and pipeline improvements. I also collaborate closely with developers to resolve issues and frequently handle Kubernetes deployment requests. I usually wrap up by documenting my work and updating Jira to keep the sprint on track.

Agile is a software development methodology that focuses on iterative development, collaboration, flexibility, and delivering working software in small, frequent increments.

## Tell me abt you CI/CD process that you have implemented.

In our organization we have Github as source code repository if any developers commit a code in source code repository jenkins pipeline will automatically triggered using Webhook as a first stage it will start pulling the code from source code repository once the code is pulled and checkedout next step building this code we use Maven as a part of this process once maven build the application, then we will check the code quality by using staticu code analysis will check application is secure or not after that we use toll App scan this is used for SAT & DAST after that we promote this Application to dev Env using ARGOCD & k8s Argco-cd is looking for k8s manifest in git repository whenever there is change once the new image is updated Argocd will look for new tag in the image using HELM chart it would deploy to new version of an application into target k8s cluster.  

## Q: Can you explain the CICD process in your current project ? or Can you talk about any CICD process that you have implemented ?

A: In the current project we use the following tools orchestrated with Jenkins to achieve CICD.
   - Maven, Sonar, AppScan, ArgoCD, and Kubernetes
   
   Coming to the implementation, the entire process takes place in 8 steps
   
    1. Code Commit: Developers commit code changes to a Git repository hosted on GitHub.
    2. Jenkins Build: Jenkins is triggered to build the code using Maven. Maven builds the code and runs unit tests.
    3. Code Analysis: Sonar is used to perform static code analysis to identify any code quality issues, security vulnerabilities,and bugs.
    4. Security Scan: AppScan is used to perform a security scan on the application to identify any security vulnerabilities.
    5. Deploy to Dev Environment: If the build and scans pass, Jenkins deploys the code to a development environment managed by Kubernetes.
    6. Continuous Deployment: ArgoCD is used to manage continuous deployment. ArgoCD watches the Git repository and automatically
    deploys new changes to the development environment as soon as they are committed.
    7. Promote to Production: When the code is ready for production, it is manually promoted using ArgoCD to the production environment.
    8. Monitoring: The application is monitored for performance and availability using Kubernetes tools and other monitoring tools.  
    
    
 ## Issues faced during LB INgress provision same thing we can frame during interview (challenges faced in recent project)  

One of the most challenging issues I recently resolved involved provisioning an AWS Application Load Balancer through the AWS Load Balancer Controller in EKS.
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

## CI/CD & Build Tools

Q: How do you design/manage a CI/CD pipeline?  
👉 Use Jenkins/Bamboo with modular stages (build → test → package → deploy). Integrate SonarQube + Nexus. Make it fast, automated, and repeatable.

Q: How do you handle build failures?  
Check logs, re-run with debug, fix root cause, notify dev team, prevent recurrence via automation/tests.


## Reduced build and deployment times by20% by optimizing CI/CDpipelines.

Earlier, our builds were slow, taking about 5 minutes. I analyzed the CI/CD pipeline and found redundant steps and lack of caching. I implemented parallel builds, introduced dependency caching, and automated deployment scripts. These optimizations reduced build and deployment times by 20%, which allowed faster feedback, quicker releases, and improved developer productivity.

Key Points to Cover

Builds and deployments were slow, causing delays in delivery.

Parallelization: Running tests/build steps in parallel instead of sequential.

Caching: Using Docker layer caching, dependency caching (Maven, npm, pip, etc.).

Optimized builds: Removed unnecessary steps, modularized jobs.

Infrastructure: Used auto-scaling agents/runners for faster builds.

Automation: Introduced Infrastructure as Code (IaC) or deployment scripts.

20% faster builds/deployments (e.g., from 5 min to 3 min).

Improved team productivity and release frequency.

## How do you ensure the security and compliance of your CI/CD pipelines in aws

I secure CI/CD pipelines by integrating DevSecOps practices:

**1: Use IAM roles and least privilege access** Assign the EC2 instance an IAM Role with minimal ECR + EKS deploy permissions.

**2: Store secrets in AWS Secrets Manager** store sensitive credentials like DB passwords, API keys, or tokens in AWS Secrets Manager, which securely encrypts them with KMS, rotates them automatically, and allows controlled access via IAM policies instead of hardcoding in code or pipelines

**3 Scan code and images for vulnerabilities** Continuously rescan images in your registry (not just at build time).

**4 Enforce policies with tools like Sentinel** Using Sentinel, you can enforce compliance/security as part of your CI/CD pipelines by defining policies as code.  

**5 Implement approval gates and audit logging** Implement approval gates by requiring manual checks before production deploys, and enable audit logging (via CloudWatch or CI/CD logs) to track who approved.

**6 Monitor pipeline activity with Cloud watch** 
Monitor pipeline activity using centralized logging and monitoring tools like AWS CloudWatch, CI/CD audit logs to track builds, deployments, failures, and security events in real time.

## How do you keep your infrastructure and application code in sync?

I keep infrastructure and application code in sync by using Infrastructure as Code with Git-based workflows, so both are version-controlled, reviewed, and deployed together through CI/CD pipelines.

## What is the difference between rebase and merge? Which one do you prefer in CI/CD workflows?

In CI/CD workflows, I prefer rebase for feature branches to keep history clean and avoid merge clutter. However, for merging into the main branch, I often use merge (via pull requests) because it preserves history and avoids rewriting commits others may depend on. A common workflow is to rebase locally for cleanup and then merge via PR into main

## How do you implement approval gates in a Jenkins pipeline (e.g., manual approval before production)?

In Jenkins, I implement approval gates using the input step in a declarative pipeline, which pauses execution and requires manual approval before moving to production. For stricter controls, I combine this with RBAC so only authorized users can approve. In enterprise setups, I also integrate Jenkins with external systems like ServiceNow or Slack for approval workflows.

## Have you handled parallel execution in a pipeline? How and why?

Yes, I’ve implemented parallel execution in Jenkins pipelines using the parallel directive. For example, I run unit tests, integration tests, and security scans at the same time, which significantly reduces pipeline runtime and provides faster feedback to developers. I prefer it in CI/CD because it optimizes resources and shortens release cycles

## How do you pass parameters between stages in a Jenkins declarative pipeline?

In Jenkins declarative pipelines, I usually pass values between stages using environment variables (env.VAR) or global Groovy variables inside script {}. If I need to pass artifacts, I use stash/unstash. For user inputs or re-runs, I rely on build parameters. This ensures data flows smoothly between pipeline stages without breaking the declarative syntax

In Jenkins, stash is used to temporarily save files from one stage, and unstash is used to retrieve them in another stage.

## We have 100 of pipeline in jenkins how the data of all these pipeline is managed by jenkins.

When we run jenkins pipeline a new workspace is created in jenkins server, it is an independent directory dedicated to each jenkins project or pipeline. 

## what are 2 type of pipeline in jenkins?

1 **Declarative Pipeline** – A simplified, structured syntax (pipeline { ... }) designed to make pipeline creation easier and more readable.

2 **Scripted Pipeline** – Uses Groovy scripting, more flexible and powerful but complex.

## How do you debug a Jenkins job stuck on “Waiting for Executor”? 

Answer: No free agents → Increase executors → Add agent nodes → Use Kubernetes dynamic agents. 

## How do you implement auto-scaling for Jenkins agents? 

Answer: Integrate Jenkins with Kubernetes plugin → Agents spin up as pods on demand → Auto-terminate after job completion.

## How do you implement CI/CD for microservices? 

Answer: Use separate pipelines per microservice → Containerize each → Deploy to Kubernetes via Helm/ArgoCD → Centralized monitoring. 

## what are best practices for managing jenkins pipeline.

Best practices include using declarative Jenkinsfiles in Git, modular shared libraries, least privilege credentials, clear stages, fail-fast, approval gates, and proper secrets management.  

## After install jenkins what are initial steps to secure jenkins?

By enabling security manage_jenkins configure Global security for authentication autherization & restrice public access.  

## what are the some common jenkins pipeline errors & how do you debug this.

Common error like synctax error missing credentials & plugin issue. By verifying console O/P and checking syntax in jenkins file.

## what would be the added benefits of cloning.

Saves time by reusing existing configuration reducing errors Easy modification of job.

## what is jenkins build artifacts.

In Jenkins, build artifacts are the output files from a build (like JARs, WARs, reports) that can be archived and used for deployment or analysis.

## How to Manage Secrets Across Environments in Kubernetes (AWS) for interview purpose in short

In Kubernetes on AWS, manage secrets per environment using AWS Secrets Manager, inject them into pods via Kubernetes Secrets or IAM Roles for Service Accounts (IRSA), and avoid hardcoding credentials, ensuring least privilege access and automated rotation

Stages in in CI/CD Pipeline.

| Build | Dev | QA | Stage | Prod |
|-------|-----|----|-------|------|
| Lower Env | → | → | Upper ENV | → |

## What kind of static code analysis have you deal with?

We dealt with developers not using the funtion cretaed a function but not invoking a function or using depricated modules, so the static check failed.

## Note after CI build success it will create a build tag same tag will be found in dockerhub.

## if we need previous all stages successful then moved to next stage 

in the last stage 
 needs: [build, docker, code-qupwality] It says that if all 3 phases passed then trigger our next stage.

## Q: What are the different ways to trigger jenkins pipelines ?

Poll SCM: Jenkins periodically checks the repo for changes and triggers a build if detected.  

Build Triggers: Jenkins Git plugin can auto-build specific branches when changes occur.  

Webhooks: GitHub webhooks notify Jenkins on code push to trigger builds instantly.    

## Q: How to backup Jenkins ?

To back up Jenkins, I back up the $JENKINS_HOME directory, which contains jobs, plugins, configs, and credentials. This can be done manually, automated with cron jobs, or using plugins like ThinBackup. In cloud setups, I use S3 sync or persistent volume snapshots for backup. Restoration simply involves restoring $JENKINS_HOME on a fresh Jenkins installation

## Q: How do you store/secure/handle secrets in Jenkins ?

In Jenkins, I store secrets in the Credentials Store (encrypted at rest), inject them at runtime with withCredentials, and for better security integrate with Vault or AWS Secrets Manager. I also enforce RBAC and rotation policies

## What is latest version of Jenkins or which version of Jenkins are you using ?

## 2.516.2 

## How do you trigger a pipeline based on a Git tag push instead of a branch commit?

To trigger on tags, we configure the pipeline trigger for tags instead of branches. For example, in GitHub Actions we use on: push: tags, in GitLab only: tags, and in Jenkins use when { buildingTag() }.

## Q: What is shared modules in Jenkins ?

Shared modules in Jenkins are Shared Libraries — centralized Groovy code stored in Git, which can be imported into pipelines to reuse common steps like build, test, and deploy across multiple projects

## Q: can you use Jenkins to build applications with multiple programming languages using different agents in different stages ?

Yes, Jenkins supports multi-language builds by using different agents per stage. For example, I can run a Java build on a Maven agent, 
a Node.js build on a Node agent, and a Python build on another agent — all in one pipeline.

## Q: How to setup auto-scaling group for Jenkins in AWS ?

To set up Jenkins with Auto Scaling in AWS, I create a Launch Template with Jenkins installed via user data, then attach it to an Auto Scaling Group behind an ALB. For persistence, I mount Jenkins home on EFS. Usually, the master stays fixed, and the Auto Scaling Group is used for Jenkins agents

## Q: How to add a new worker node in Jenkins ?

A: Log into the Jenkins master and navigate to Manage Jenkins > Manage Nodes > New Node. Enter a name for the new node and select Permanent Agent. Configure SSH and click on Launch.

## Q: How to add a new plugin in Jenkins ?

A: Using the CLI, 
   `java -jar jenkins-cli.jar install-plugin <PLUGIN_NAME>`
  
  Using the UI,

   1. Click on the "Manage Jenkins" link in the left-side menu.
   2. Click on the "Manage Plugins" link.

## Q: What is JNLP and why is it used in Jenkins ?

JNLP (Java Network Launch Protocol) in Jenkins is used for agents to connect back to the master. It’s mainly used when the master cannot directly reach the agent (e.g., due to firewalls or private networks). The agent pulls the connection using a JNLP token, which makes it secure and firewall-friendly.

 ## Explain about jenkins file.

 Jenkins file is text file it conatins the steps of pipeline for build test & deploy.

 ## what is parameter in jenkins.

 Parameter is special kind of variable , while working with jenkins jobs we often need to pass some parameter to the code being exuted when we trigger jobs.  

 ## below are parameter in jenkins  

  1 Boolean  2 choice 3 Credentials 4 file 5 string 6 password  

  ## Where is jenkins working directory.

  Jenkins working directory under JENKINS_HOME directory its is root directory of jenkins where jenkins installed jenkins uses to perform build & archives.

 ## Release Management

Q: Walk me through a release process.
👉 Code freeze → Build & test → Store artifact (Nexus) → Deploy QA → CAB approval → Prod deploy (blue-green/canary) → Monitoring → Post-release report.

Q: How do you ensure releases are stable/repeatable?
👉 Automate pipelines, use immutable artifacts, IaC for envs, integrate tests, approval gates.

Q: Rollback strategy?
👉 Redeploy last stable artifact, use IaC for infra rollback, blue-green/canary for easy switchback.

## Q: What are some of the common plugins that you use in Jenkins ?

The common Jenkins plugins I use are Git, Pipeline, Docker, SonarQube, and Slack Notification.


Below is for python use docker to build an image.
 pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git credentialsId: 'f87a34a8-0e09-45e7-b9cf-6dc68feac670', 
                url: 'https://github.com/iam-veeramalla/cicd-end-to-end',
                branch: 'main'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t abhishekf5/cicd-e2e:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    docker push abhishekf5/cicd-e2e:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: 'f87a34a8-0e09-45e7-b9cf-6dc68feac670', 
                url: 'https://github.com/iam-veeramalla/cicd-demo-manifests-repo.git',
                branch: 'main'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    withCredentials([usernamePassword(credentialsId: 'f87a34a8-0e09-45e7-b9cf-6dc68feac670', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh '''
                        cat deploy.yaml
                        sed -i '' "s/32/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/iam-veeramalla/cicd-demo-manifests-repo.git HEAD:main
                        '''                        
                    }
                }
            }
        }
    }

