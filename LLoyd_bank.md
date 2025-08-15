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



