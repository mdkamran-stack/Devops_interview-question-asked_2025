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

##Question: What is Azure App service.  

Azure App Service is a fully managed PaaS that lets you build, deploy, and scale web apps and APIs quickly without managing infrastructure, with built-in CI/CD, scaling, and security.