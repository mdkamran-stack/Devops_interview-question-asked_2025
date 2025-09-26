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
