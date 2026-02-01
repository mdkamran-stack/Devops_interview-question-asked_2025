# This commit is for questions asked me as devops interview in 2025

## Pepsico

## What is output of Maven Build, if it is jar file or war file what command will put in docker file?

The output produced jar or war file.

``` JAR we use java -jar app.jar, and for WAR we deploy it in Tomcat and run with catalina.sh run.``` 

## scenario: you have one repo and want to build a pipeline to deploy to AWS (e.g., ECS, EKS, 

``` In a single repo, I build a pipeline where a code commit triggers a build, a Docker image is created and pushed to ECR, and then deployed to AWS (ECS/EKS/Beanstalk) using IaC or manifest files from the same repo. ```  

## how is your application is connected in frontend in your cluster

In Kubernetes, my frontend connects to the backend through a Service or Ingress. The Service exposes the backend pods, and Ingress (or LoadBalancer) provides external access with DNS so frontend can reach it reliably.  

## authentication and autherization who will manage application or loadbalancer ? 

Authentication and authorization are mainly handled by the application or an identity provider (like IAM, Okta, Keycloak). Load balancers can help with SSL termination or simple auth, but detailed access control is managed at the app level.  

## how it will authenticate weather user is valid or not  

JWT (JSON Web Token) is a compact token used for authentication, containing a header, payload, and signature. The server issues it after login, and the client sends it with requests to prove identity.

## what is HIGH IO usage 

High I/O usage occurs when processes perform excessive reads/writes to disks or network, potentially causing slow performance. It’s monitored using tools like iostat.  

## how to export metrics to prometeus? 

##  lets say you have application java running micro services in kubernetes cluster from there to grafana


3 instance in 3 diff regions 

3 ec3 instance load from external source i have to divide traffic equally on all 3 

git stash 

shared lib in jenkins 

Bristlecone interview
------------------------
create 10 ubnut server with apache installations  /var/lib/html via terraform  
write shell script get the process with user name kamran and kill the process  
how to deploy application in k8s   
how to setup k8s in eks  
types of loadbalancer in k8s  
how to setup hpa na vpa  
what is selinux   
what is loadbalncer in aws & how to setup app LB and NW LB , how network LB works  
What is the difference between the count and for_each meta-arguments?  
how to communicate with two infrastructure?
how to migrate on-perm to cloud?
how to communicate microservices?
