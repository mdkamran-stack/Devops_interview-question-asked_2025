# LLyods bank asked questions.

##  How Pod autoscale.

HPA in kubernetes automatically adjust the number of pod replicas in a deployment, replica set, or stateful set based on cpu utilization, memory, or custom metrics.
This ensures the application scales up under high load and scale down during low usage, optimizing cost and performance.

## Basic HPA Example (YAML)
```yml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: myapp-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: myapp-deployment
  minReplicas: 2
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

## Explanation:
a. minRelicas /maxRelicas -> range for scaling  
b. metrics -> scaling cond ( e.g, CPU usage > 50 %)  
c. works with metrics-server in cluster  

## Question 2: "Secrets across environments" in Azure DevOps / Kubernetes context.
Let’s break it down simply for interview prep.  

When we deploy app across Dev, QA , Stage , Prod , we often need different secret (DB passwords, API keys, connection strings )  
we should never hardcoded  

## Best Practices.

1 Use Azure Key Vault  
 . store sceret for each environment in seprate key vault instances or naming convention (e.g, db-password-dev , db-password-prd).  
 . create key vault Service connections in azure devops for each environment.  
 . Link key vault to a pipeline using AzureKeyvault@2 task.  
 
 2. use variable Group in Azure Devops   
  . Create variable group for each environment, like them to key vault or manually add secrets.  
  . Lock variable as sceret so they are masked in logs.  

 3. USe K8s secrets (for AKS)  
    . create seprate secrets per namespace/environment:  
    ``` bash
    kubectl create secret generic db-secret --from-literal=DB_PASSWORD=pass123 -n dev
    ```  
    . Referenced them in elm chart or manifests.  
  4. RBAC and Access Policies  
    . Use least privilege - Dev secret accessible to dev team, prod secrets locked down.  
    . Enable audit log on key vault.  

    ## Azure DevOps YAML Example with Key Vault  
```yaml
variables:
- group: DevSecrets

steps:
- task: AzureKeyVault@2
  inputs:
    azureSubscription: 'MyServiceConnection'
    KeyVaultName: 'my-dev-kv'
    SecretsFilter: '*'
    RunAsPreJob: true
```  
## Question 3 If is prod deploy failing how troubeshoot.  

### 1 Check pipeline Logs
  . identify which stage failed (build, test,deploy)  
  . Looks for specific error message and failed task.  
### 2 Verify service connections  
  . Ensure Azure service connection is valid and cred haven't expired.
### 3 Validate deployment artifacts.  
  .  check if build artifacts are generated and match expected version.  
  . confirm integrity of container images or packages.  
### 4 check infrastucture readiness  
  . validate target env availability (App Service , AKS , VM).  
  . verify network connectivity and firewall rules.  
### 5 Review secrets and configurations.  
  . Ensure environment variables, secrets and conf file are corrected for pod.  
### 6 check deployment strategies.  
   . if using Blud-Green /canary, verify routing and rollback points.  
### 7 Use azure monitoring & App insights.  
    . check logs, metrics, and alert to identify and rollback issues.  
### 8 Rollback if Needed  
     . Use last successful build or inrastructure snapshort to restore service.  
### 💡 Quick 10-sec interview answer:

“I’d start by checking the Azure DevOps pipeline logs to find the failing step, then verify service connections, artifacts, and production configurations. I’d also check Azure Monitor/App Insights for runtime errors. If I can’t resolve quickly, I’d rollback to the last working deployment and then debug in a safe environment.”

## ques 4 : how did you check Health of conatiner configuration in k8s.  

### Types of Health checks.  
#### 1. Liveness Probe: Ensure if the container is alive , if fails , k8s restart the conatiner.  
#### 2. Readiness Probe : checks if the container is ready ti accepts, if it fails, traffic is not sent to it.  
#### 3. Startup Probe : Ensures slow-starting apps get enough time before k8s starts liveness checks.  
 ```yaml

apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: myapp
    image: myapp:latest
    ports:
    - containerPort: 8080
    livenessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 10
      periodSeconds: 5
    readinessProbe:
      httpGet:
        path: /ready
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 5

```
### How it works  
. LivenessProbe -> if /health fails repetadely, pod is restarting.
. readinessProbe -> if /ready  fails, pod stays running but is removed the service load balancer.  
. Startup Probe -> (Optional) Prevents liveness from killing slow-starting apps.  
 
 💡 Quick Interview Answer:

“In Kubernetes, I configure liveness, readiness, and startup probes in the pod spec. Liveness ensures the container is restarted if unresponsive, readiness ensures it only gets traffic when ready, and startup helps with slow initializations. This keeps the application stable and reliable.”