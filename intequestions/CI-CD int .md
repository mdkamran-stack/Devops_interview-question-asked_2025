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

# Service outage user-report 
When a user cannot access the application, I first determine the scope and impact of the issue. Then I check monitoring dashboards, DNS resolution, load balancer health, Kubernetes resources such as Ingress, Services, and Pods, and review application logs. I also verify whether any recent deployments or configuration changes were made. If a recent change caused the issue, I perform a rollback to restore service quickly and then conduct a root cause analysis to prevent recurrence.

P1 ticket 
=============
Confirm Severity and Business Impact
Validate whether it truly meets P1 criteria:
Production outage
Revenue-impacting issue
Security/data integrity risk
Critical SLA breach  Quickly assess:

No of users affected Entire app is down or some functionalities not working my focusing the on rapid service restoration before root-cause analysis. I establish clear ownership through incident command roles, prioritize customer-facing impact above all else, communicate updates consistently to stakeholders,

E-commerce site they sells pharma.

I would like to understand how success is measured for this role. What would be the key expectations and milestones 
for the selected candidate in the first 3 months, 6 months, and over the first year?  

Could you share a bit more about the background of this role? Is this position part of team expansion and new initiatives, 
or is it a backfill for an existing role?   

# CAP Theorem
“In distributed systems, CAP theorem states that a system can provide only two out of three guarantees: Consistency, Availability, and Partition Tolerance.

Consistency means all users see the same latest data.  
Availability means the system always responds to requests.  
Partition tolerance means the system continues working even during network failures between nodes.  

During a network partition, the system must choose either Consistency or Availability.  

For example:

Banking systems usually prefer CP to avoid incorrect transactions.  
Social media applications usually prefer AP for high availability.”  
Optimization of Jenkins pipeline using paralleism  

Both the stages start at same time so our build time may optimize 

Pipeline {  
 agent any   
   stages {  
     stage ('parallelStage')  
        parallel {  
          stage('Stage1') {  
            steps {  
              echo 'Stage 1 started'  
              sleep(time: 5, unit: SECONDS') 
              echo 'Stage 1 completed'   
              }  
            }  
            stage('Stage2') {  
              steps {  
                echo 'Stage 2 started'  
                sleep(time: 5, unit: SECONDS')  
                echo 'Stage 2 completed'  
               }  
             }  
          }  
       }  
    }  
                
 # StatefulSet Pods Failing Due to Volume Affinity Scenraio

### Scenario
You are running a MongoDB StatefulSet with three replicas.  
You need to scale it to five replicas to handle a data surge.

However, Pod-3 and Pod-4 are stuck in Pending state with:

```text
FailedScheduling: 0/3 nodes are available:
3 node(s) had volume node affinity conflict.
```

### Question
How can a cluster have enough CPU/Memory but still fail to schedule a StatefulSet pod due to storage affinity?

### Answer
- This usually happens in Multi-AZ Kubernetes clusters.
- The Persistent Volume (PV) may be attached to Zone-A.
- But available worker nodes/resources are in Zone-B.
- Kubernetes must schedule the pod in the same zone where the volume exists.
- Because of this zone mismatch, scheduling fails even though CPU and memory are available.

### Solution
Use `WaitForFirstConsumer` in the StorageClass:

```yaml
volumeBindingMode: WaitForFirstConsumer
```

This ensures the Persistent Volume is created in the same zone where the pod gets scheduled.

## Tell me about your self.
Hello, my name is Md Kamran. I graduated in 2015 and have over 9 years of overall IT experience.

I began my career as a Linux Administrator, where I supported multiple clients and was responsible for maintaining system stability, resolving performance bottlenecks, and handling server migrations. I was actively involved in upgrade projects, moving systems from legacy versions to newer, more secure, and stable environments.

Over time, I transitioned into a Cloud and DevOps Engineer role. In this capacity, I have worked extensively with modern DevOps tools and cloud technologies   
I have experience building CI/CD pipelines, automating infrastructure provisioning, deploying containerized applications, and managing scalable cloud-native environments.i have built like reusable terraform module for vpc , eks RDS application lB IM and automated provisioning pipeline with S3 backend and dynamodb for state locking now currently known as TF lock in like nwere version and also handling drift troubleshooting production state and managing multiple environments setup using wrkspaces i worked extensively in k8s designing clusters managing deployments, statefulset demon sets network policies and debugging like crashloopback off scheduling failures rbac misconfiguration I am confortable tools like prometheus grafana & cloudwatch , implemeted jenkins and github action for zero touch deployment integrated with static scan like sonar qube, owas dependency check and contenarize appwith docker and implemeted blue green and canary deployment we have multiple respository in our org we have multiple microservices like all application deployed to AWS which is our primary cloud platform , overall my cloud and devops experience like a blend of hands on engineering production troubleshooting automation and designing scalbale and highly availabe & secure systems my major activity like writing infrasctructure as code using TF where i work closely with devlopment teams where they request for infrastructure creations whenver request come to devops team i am one of the team members who pickup the tf related activities and or iac and delivers work through TF personally i build TF code in the modular approach so i also work in TF modules and putting them in the centralized location.

AWS for designing and managing cloud infrastructure

We use Prometheus to monitor Kubernetes and application-level metrics, while CloudWatch is used to monitor AWS infrastructure services like EC2, RDS, and load balancers. Grafana is used to visualize metrics from both sources.


verall, my focus has always been on automation, reliability, performance optimization, and delivering secure, scalable infrastructure solutions and monitoing tool like cloudwatch & promethus and grafana ||
I work closely with development and security teams to ensure secure, scalable, and reliable deployments. I also handle monitoring, logging, incident response, and continuous improvement of platform reliability
coming to cloud technologies i am having very good experince with AWS , mostly in AWS i have i have used services like EC2 , vpc , aws lambda . s3 , EKS , cloud trail , guradduty , AWS WAF & few other services creating and maintaing github repositories, as well coming to my project  

## Day to day activities:

As a DevOps Engineer in an Agile environment, I start by checking monitoring dashboards and CI/CD pipelines, then join daily stand-ups to align with sprint goals. During the day, I focus on backlog items such as infrastructure automation, deployments, and pipeline improvements. I also collaborate closely with developers to resolve issues and frequently handle Kubernetes deployment requests. I usually wrap up by documenting my work and updating Jira to keep the sprint on track.

Agile is a software development methodology that focuses on iterative development, collaboration, flexibility, and delivering working software in small, frequent increments.

# How to integrate load testing in ci cd pipeline  (Ispace asked)

💡 Interview Answer (Best)

“I integrate load testing into CI/CD by using tools like k6 or JMeter, executing tests as a pipeline stage after deployment to staging. I define performance thresholds, and if they are breached, the pipeline fails. This ensures performance regressions are caught early before production release.”

##  During a deployment, your CI/CD pipeline fails unexpectedly. What approach would you take to troubleshoot and fix the problem?

I first contain the impact, identify the failing stage using logs, isolate whether it’s code, config, infra, or dependency related, apply a minimal fix, redeploy safely, and then add preventive checks to avoid recurrence.

## Q: Can you explain the CICD process in your current project ? or Can you talk about any CICD process that you have implemented ?

## CI/CD Pipeline Flow (With Docker Image Build & GitOps)##  

---

## 1. Git Checkout
Jenkins checks out the latest source code from GitHub repository.

---

## 2. Compile Stage
Maven compiles the application source code.

```bash
mvn clean compile
```

---

## 3. Test Stage
Unit tests are executed to validate application functionality.

```bash
mvn test
```

---

## 4. TruffleHog Security Scan
TruffleHog scans source code repository for:
- Hardcoded secrets
- API keys
- Passwords
- SSH keys
- Tokens

If secrets are detected, pipeline fails immediately.

---

## 5. SonarQube Analysis
SonarQube performs static code analysis to identify:
- Bugs
- Vulnerabilities
- Code smells
- Technical debt

---

## 6. Quality Gate Validation
Pipeline validates SonarQube quality gate.

If quality gate fails:
```text
Deployment stops automatically.
```

---

## 7. Build Stage
Maven packages the application and generates artifact:
- JAR
- WAR

```bash
mvn package
```

---

## 8. Publish to Nexus
Artifact is uploaded to Nexus repository for:
- Centralized artifact management
- Version control
- Reusability

---

## 9. Docker Build & Tag
Docker image is created using Dockerfile and tagged with build number/version.

```bash
docker build -t app:v1 .
```

---

## 10. Trivy Image Scan
Trivy scans Docker image for:
- OS vulnerabilities
- Library/package vulnerabilities
- Secrets
- Docker misconfigurations

```bash
trivy image --exit-code 1 --severity CRITICAL,HIGH app:v1
```

Pipeline fails if critical vulnerabilities are found.

---

## 11. Push Docker Image
Docker image is pushed to container registry such as:
- DockerHub
- AWS ECR

---

## 12. Deploy to Kubernetes
Application is deployed to Kubernetes cluster using:
- Helm charts
OR
- kubectl manifests

```bash
helm upgrade --install myapp ./helm-chart
```

---

## 13. Deployment Verification
Post-deployment validation checks:
- Pod health
- Readiness probes
- Application accessibility

Commands:
```bash
kubectl get pods
kubectl get svc
```

---

## 14. Monitoring & Alerting
After deployment, monitoring tools are used to track:
- Application health
- CPU/Memory usage
- Pod status
- API latency
- Error rates

Common tools:
- Prometheus
- Grafana
- CloudWatch
- ELK Stack

Alerts are configured for:
- Pod failures
- High resource usage
- Application downtime
- Error spikes

---
## 15. Post Actions
Pipeline sends:
- Email notifications
- Slack alerts
- Build status updates

## CI/CD & Build Tools

Q: How do you design/manage a CI/CD pipeline?  
👉 Use Jenkins with modular stages (build → test → package → deploy). Integrate SonarQube + Nexus. Make it fast, automated, and repeatable.

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

20% faster builds/deployments (e.g., from 30 min to 3 min).

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

## SonarQube
SonarQube is a code quality and security analysis tool that scans source code to detect bugs, vulnerabilities, and code smells, and helps maintain clean and secure code in CI/CD pipelines.  

## Quality Gates
A Quality Gate is a set of conditions in SonarQube that determines whether code passes or fails quality checks based on metrics like bugs, vulnerabilities, and coverage before allowing deployment  

## What is the difference between rebase and merge? Which one do you prefer in CI/CD workflows?

In CI/CD workflows, I prefer rebase for feature branches to keep history clean and avoid merge clutter. However, for merging into the main branch, I often use merge (via pull requests) because it preserves history and avoids rewriting commits others may depend on. A common workflow is to rebase locally for cleanup and then merge via PR into main

## How do you implement approval gates in a Jenkins pipeline (e.g., manual approval before production)?

In Jenkins, I implement approval gates using the input step in a declarative pipeline, which pauses execution and requires manual approval before moving to production. For stricter controls, I combine this with RBAC so only authorized users can approve. In enterprise setups, I also integrate Jenkins with external systems like JiRA or Slack for approval workflows.

##  What are quality gates? Or, how do you confirm that a built artifact is good?

Quality gates are automated approval checkpoints that ensure an artifact meets defined standards for code quality, security, testing, and compliance before promotion.

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

## What is Agent in jenkins
A jenkins Agent is worker machine where the jobs (build,test,deploy) actually run.  
it has two part
Master/controller > manages jobs.  
Agent - Hands (execute jobs)  
Agents help by Distributing workload 2 Running jobs on different OS (linux win mac) 3 Running jobs in parallel 4 scaling for large project  

## Needs of Agent.

suppose we have java based app , python based app So each of env need specific agent in order to segregate env we need agent.

## Types of Agent
1 Builtin agent runs on the same jenkins m/c  
2 Static Agent - A fixed m/c connected to jekins  
3 Dynamic Agent - Created on demand (E.G: Docker, Kubernetes, Cloud )

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

