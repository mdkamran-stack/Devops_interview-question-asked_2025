# This commit is for questions asked me as devops interview in 2025

## Pepsico

## What is output of Maven Build, if it is jar file or war file what command will put in docker file?

The output produced jar or war file.

``` JAR we use java -jar app.jar, and for WAR we deploy it in Tomcat and run with catalina.sh run.``` 

## scenario: you have one repo and want to build a pipeline to deploy to AWS (e.g., ECS, EKS, 

``` In a single repo, I build a pipeline where code commit triggers a build, Docker image is created and pushed to ECR, then deployed to AWS 
(ECS/EKS/Beanstalk) using IaC or manifest files from the same repo```

