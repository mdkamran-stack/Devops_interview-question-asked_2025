## Tell me abt you CI/CD process that you have implemented.

In our organization we have Github as source code repository if any developers commit a code in source code repository jenkins pipeline will automatically triggered using Webhook as a first stage it will start pulling the code from source code repository once the code is pulled and checkedout next step building this code we use Maven as a part of this process once maven build the application, then we will check the code quality by using staticu code analysis will check application is secure or not after that we use toll App scan this is used for SAT & DAST after that we promote this Application to dev Env using ARGOCD & k8s Argco-cd is looking for k8s manifest in git repository whenever there is change once the new image is updated Argocd will look for new tag in the image using HELM chart it would deploy to new version of an application into target k8s cluster.

## How do you ensure the security and compliance of your CI/CD pipelines in aws

I secure CI/CD pipelines by integrating DevSecOps practices:

**1: Use IAM roles and least privilege access** Assign the EC2 instance an IAM Role with minimal ECR + EKS deploy permissions.

**2: Store secrets in AWS Secrets Manager** store sensitive credentials like DB passwords, API keys, or tokens in AWS Secrets Manager, which securely encrypts them with KMS, rotates them automatically, and allows controlled access via IAM policies instead of hardcoding in code or pipelines

**3 Scan code and images for vulnerabilities** Continuously rescan images in your registry (not just at build time).

**4 Enforce policies with tools like Sentinel** Using Sentinel, you can enforce compliance/security as part of your CI/CD pipelines by defining policies as code.  

**5 Implement approval gates and audit logging** Implement approval gates by requiring manual checks before production deploys, and enable audit logging (via CloudWatch or CI/CD logs) to track who approved.

**6 Monitor pipeline activity with Cloud watch** Monitor pipeline activity using centralized logging and monitoring tools like AWS CloudWatch, CI/CD audit logs to track builds, deployments, failures, and security events in real time.

## We have 100 of pipeline in jenkins how the data of all these pipeline is managed by jenkins.

When we run jenkins pipeline a new workspace is created in jenkins server, it is an independent directory dedicated to each jenkins project or pipeline. 

## what are 2 type of pipeline in jenkins?

In jenkins 2 types of pipeline is cripted pipeline & declarative pipeline.

Scripted pipelines are Groovy-based and flexible but complex, while Declarative pipelines use a structured syntax (pipeline {}) that’s simpler, more readable, and recommended.

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

## Q: Can you explain the CICD process in your current project ? or Can you talk about any CICD process that you have implemented ?

A: In the current project we use the following tools orchestrated with Jenkins to achieve CICD.
   - Maven, Sonar, AppScan, ArgoCD, and Kubernetes
   
   Coming to the implementation, the entire process takes place in 8 steps
    
    1. Code Commit: Developers commit code changes to a Git repository hosted on GitHub.
    2. Jenkins Build: Jenkins is triggered to build the code using Maven. Maven builds the code and runs unit tests.
    3. Code Analysis: Sonar is used to perform static code analysis to identify any code quality issues, security vulnerabilities, and bugs.
    4. Security Scan: AppScan is used to perform a security scan on the application to identify any security vulnerabilities.
    5. Deploy to Dev Environment: If the build and scans pass, Jenkins deploys the code to a development environment managed by Kubernetes.
    6. Continuous Deployment: ArgoCD is used to manage continuous deployment. ArgoCD watches the Git repository and automatically deploys new changes to the development environment as soon as they are committed.
    7. Promote to Production: When the code is ready for production, it is manually promoted using ArgoCD to the production environment.
    8. Monitoring: The application is monitored for performance and availability using Kubernetes tools and other monitoring tools.
   

## Q: What are the different ways to trigger jenkins pipelines ?

Poll SCM: Jenkins periodically checks the repo for changes and triggers a build if detected.  

Build Triggers: Jenkins Git plugin can auto-build specific branches when changes occur.  

Webhooks: GitHub webhooks notify Jenkins on code push to trigger builds instantly.    

## Q: How to backup Jenkins ?

A: Backing up Jenkins is a very easy process, there are multiple default and configured files and folders in Jenkins that you might want to backup.
```  
  - Configuration: The `~/.jenkins` folder. You can use a tool like rsync to backup the entire directory to another location.
  
    - Plugins: Backup the plugins installed in Jenkins by copying the plugins directory located in JENKINS_HOME/plugins to another location.
    
    - Jobs: Backup the Jenkins jobs by copying the jobs directory located in JENKINS_HOME/jobs to another location.
    
    - User Content: If you have added any custom content, such as build artifacts, scripts, or job configurations, to the Jenkins environment, make sure to backup those as well.
    
    - Database Backup: If you are using a database to store information such as build results, you will need to backup the database separately. This typically involves using a database backup tool, such as mysqldump for MySQL, to export the data to another location.
```
One can schedule the backups to occur regularly, such as daily or weekly, to ensure that you always have a recent copy of your Jenkins environment available. You can use tools such as cron or Windows Task Scheduler to automate the backup process.

## Q: How do you store/secure/handle secrets in Jenkins ?

A: Again, there are multiple ways to achieve this, 
   Let me give you a brief explanation of all the posible options.
```  
   - Credentials Plugin: Jenkins provides a credentials plugin that can be used to store secrets such as passwords, API keys, and certificates. The secrets are encrypted and stored securely within Jenkins, and can be easily retrieved in build scripts or used in other plugins.
   
   - Environment Variables: Secrets can be stored as environment variables in Jenkins and referenced in build scripts. However, this method is less secure because environment variables are visible in the build logs.
   
   - Hashicorp Vault: Jenkins can be integrated with Hashicorp Vault, which is a secure secrets management tool. Vault can be used to store and manage sensitive information, and Jenkins can retrieve the secrets as needed for builds.
   
   - Third-party Secret Management Tools: Jenkins can also be integrated with third-party secret management tools such as AWS Secrets Manager, Google Cloud Key Management Service, and Azure Key Vault.
```

## Q: What is latest version of Jenkins or which version of Jenkins are you using ?

## 2.516.2 

## Q: What is shared modules in Jenkins ?

A: Shared modules in Jenkins refer to a collection of reusable code and resources that can be shared across multiple Jenkins jobs. This allows for easier maintenance, reduced duplication, and improved consistency across multiple build processes.
   For example, shared modules can be used in cases like:
```
        - Libraries: Custom Java libraries, shell scripts, and other resources that can be reused across multiple jobs.
        
        - Jenkinsfile: A shared Jenkinsfile can be used to define the build process for multiple jobs, reducing duplication and making it easier to manage the build process for multiple projects.
        
        - Plugins: Common plugins can be installed once as a shared module and reused across multiple jobs, reducing the overhead of managing plugins on individual jobs.
        
        - Global Variables: Shared global variables can be defined and used across multiple jobs, making it easier to manage common build parameters such as version numbers, artifact repositories, and environment variables.
```

## Q: can you use Jenkins to build applications with multiple programming languages using different agents in different stages ?

A: Yes, Jenkins can be used to build applications with multiple programming languages by using different build agents in different stages of the build process.

Jenkins supports multiple build agents, which can be used to run build jobs on different platforms and with different configurations. By using different agents in different stages of the build process, you can build applications with multiple programming languages and ensure that the appropriate tools and libraries are available for each language.

For example, you can use one agent for compiling Java code and another agent for building a Node.js application. The agents can be configured to use different operating systems, different versions of programming languages, and different libraries and tools.

Jenkins also provides a wide range of plugins that can be used to support multiple programming languages and build tools, making it easy to integrate different parts of the build process and manage the dependencies required for each stage.

Overall, Jenkins is a flexible and powerful tool that can be used to build applications with multiple programming languages and support different stages of the build process.

## Q: How to setup auto-scaling group for Jenkins in AWS ?

A: Here is a high-level overview of how to set up an autoscaling group for Jenkins in Amazon Web Services (AWS):
```
    - Launch EC2 instances: Create an Amazon Elastic Compute Cloud (EC2) instance with the desired configuration and install Jenkins on it. This instance will be used as the base image for the autoscaling group.
    
    - Create Launch Configuration: Create a launch configuration in AWS Auto Scaling that specifies the EC2 instance type, the base image (created in step 1), and any additional configuration settings such as storage, security groups, and key pairs.
    
    - Create Autoscaling Group: Create an autoscaling group in AWS Auto Scaling and specify the launch configuration created in step 2. Also, specify the desired number of instances, the minimum number of instances, and the maximum number of instances for the autoscaling group.
    
    - Configure Scaling Policy: Configure a scaling policy for the autoscaling group to determine when new instances should be added or removed from the group. This can be based on the average CPU utilization of the instances or other performance metrics.
    
    - Load Balancer: Create a load balancer in Amazon Elastic Load Balancer (ELB) and configure it to forward traffic to the autoscaling group.
    
    - Connect to Jenkins: Connect to the Jenkins instance using the load balancer endpoint or the public IP address of one of the instances in the autoscaling group.
    
    - Monitoring: Monitor the instances in the autoscaling group using Amazon CloudWatch to ensure that they are healthy and that the autoscaling policy is functioning as expected.

 By using an autoscaling group for Jenkins, you can ensure that you have the appropriate number of instances available to handle the load on your build processes, and that new instances can be added or removed automatically as needed. This helps to ensure the reliability and scalability of your Jenkins environment.
```

## Q: How to add a new worker node in Jenkins ?

A: Log into the Jenkins master and navigate to Manage Jenkins > Manage Nodes > New Node. Enter a name for the new node and select Permanent Agent. Configure SSH and click on Launch.

## Q: How to add a new plugin in Jenkins ?

A: Using the CLI, 
   `java -jar jenkins-cli.jar install-plugin <PLUGIN_NAME>`
  
  Using the UI,

   1. Click on the "Manage Jenkins" link in the left-side menu.
   2. Click on the "Manage Plugins" link.

## Q: What is JNLP and why is it used in Jenkins ?

A: In Jenkins, JNLP is used to allow agents (also known as "slave nodes") to be launched and managed remotely by the Jenkins master instance. This allows Jenkins to distribute build tasks to multiple agents, providing scalability and improving performance.

   When a Jenkins agent is launched using JNLP, it connects to the Jenkins master and receives build tasks, which it then executes. The results of the build are then sent back to the master and displayed in the Jenkins user interface.

 ## Explain about jenkins file.

 Jenkins file is text file it conatins the steps of pipeline for build test & deploy.

 ## what is parameter in jenkins.

 Parameter is special kind of variable , while working with jenkins jobs we often need to pass some parameter to the code being exuted when we trigger jobs.  

 ## below are parameter in jenkins  

  1 Boolean  2 choice 3 Credentials 4 file 5 string 6 password  

  ## Where is jenkins working directory.

  Jenkins working directory under JENKINS_HOME directory its is root directory of jenkins where jenkins installed jenkins uses to perform build & archives.

 

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

