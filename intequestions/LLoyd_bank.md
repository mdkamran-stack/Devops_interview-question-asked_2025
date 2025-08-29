# LLyods bank asked questions.
## How do you ensure the security and compliance of your CI/CD pipelines in aws

## 🔐 Security & Compliance in AWS CI/CD Pipelines
✅ 1. Shift Left with DevSecOps
Integrate security early in the pipeline (code → build → deploy)

Use pre-commit hooks, IDE plugins, and static code analysis to catch issues before code enters the pipeline

## 🔒 2. Secure Source Control
Use AWS CodeCommit or GitHub Enterprise with private repos

Enforce least privilege IAM policies for repo access

Enable MFA and audit access logs via CloudTrail

## 🧠 3. Secrets Management
Store credentials in AWS Secrets Manager or Parameter Store

Inject secrets securely into build environments using IAM roles

Avoid hardcoding secrets in code or pipeline configs

## 🛡️ 4. Policy-as-Code & Compliance Enforcement
Use CloudFormation Guard, OPA/Gatekeeper, or Terraform Sentinel to enforce compliance rules

Validate infrastructure against CIS benchmarks, PCI-DSS, or SOC 2 requirements

Automate evidence collection with AWS Audit Manager

## 🔍 5. Vulnerability Scanning
Run SAST (Static Analysis) and DAST (Dynamic Analysis) tools during build

Scan container images with Amazon Inspector, Trivy, or Snyk

Fail builds if critical vulnerabilities are found

## 📦 6. Secure Artifact & Image Handling
Use Amazon ECR with image scanning and lifecycle policies

Sign and verify images before deployment

Restrict image sources to trusted registries

## 🔄 7. Controlled Deployments
Use AWS CodePipeline, CodeBuild, and CodeDeploy with IAM roles

Implement approval gates before production deployments

Use blue/green or canary strategies to reduce blast radius

## 📊 8. Monitoring & Auditing
Enable CloudTrail, CloudWatch Logs, and GuardDuty

Track pipeline activity and detect anomalies
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

## How to Manage Secrets Across Environments in Kubernetes (AWS)
✅ 1. Use External Secret Managers
Instead of storing secrets directly in Kubernetes, use centralized tools like:

AWS Secrets Manager

AWS Systems Manager Parameter Store

External Secrets Operator (ESO) to sync secrets into Kubernetes2

These tools allow:

Centralized secret storage

Fine-grained IAM access control

Automatic rotation and auditing

⚙️ 2. Integrate with Kubernetes via CSI Driver or ESO
Use the Secrets Store CSI Driver with AWS Secrets and Configuration Provider (ASCP) to mount secrets directly into pods. Or use External Secrets Operator to fetch secrets from AWS and inject them into Kubernetes as native secrets.

Example flow:

Define a SecretStore CRD with AWS credentials

Create an ExternalSecret that maps AWS secrets to Kubernetes secrets

ESO syncs and updates secrets automatically

## 🧠 3. Environment-Specific Secret Mapping
Use naming conventions or labels to separate secrets per environment:

yaml
externalSecret:
  name: db-credentials
  secretStoreRef:
    name: aws-prod-store
  data:
    - remoteRef:
        key: prod/db/password
      secretKey: password
For dev/staging, use different keys or stores:

dev/db/password

staging/db/password

## 🔒 4. Secure Access with IAM Roles
Use IAM Roles for Service Accounts (IRSA) to ensure only specific pods can access specific secrets. This enforces least privilege and isolates environments.

## 🔄 5. Enable Secret Rotation
AWS Secrets Manager supports automatic rotation. Combine this with ESO or CSI Driver’s rotation reconciler to keep secrets fresh inside Kubernetes.

## 📊 6. Audit and Monitor
Use AWS CloudTrail to track secret access

Monitor Kubernetes secret usage with logging tools like Falco or Audit Logs 

## How to Troubleshoot a Failed Prod Deployment in Kubernetes

A: When a production deployment fails, I follow a structured approach:

## 1. Check Deployment Status
bash
`kubectl rollout status deployment/<name>`
`kubectl describe deployment <name>`
This shows rollout progress and any events like image pull errors or failed probes.

## 2. Inspect Pod Health
```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name> -c <container-name>
```
Look for:

## CrashLoopBackOff: app crash or misconfig

## ImagePullBackOff: bad image tag or registry access

## Pending: scheduling issues or resource limits

## 3. Validate Configuration
Check environment variables, secrets, and configMaps

Ensure correct resource requests/limits

Confirm volume mounts and PVC bindings

## 4. Check Networking
Verify service selectors match pod labels

Test DNS resolution inside pods

Confirm Ingress rules and controller status

## 5. Roll Back if Needed
   ```bash
kubectl rollout undo deployment/<name>
Quickly restores the last working version.
```
## 6. Monitor and Alert
Use Prometheus/Grafana or CloudWatch for metrics

Check alerting systems for anomalies

Review audit logs for recent changes

Bonus: In AWS, I also check:

EKS node health and autoscaling

IAM roles and IRSA bindings

Load balancer status and target group health

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

## Ques: In Kubernetes, How the auto-healing mechanism automatically detects and recovers from failed workloads without manual intervention.  

### How Auto Healing works.  

1  **Liveness Probes**  
 . If a container becomes unresponsive or unhealthy, the **liveness Probe** triggers at restart.  
2  **ReplicaSet/Deployment Controller**  
  . Ensure the desired no.of pod always running.  
  . if a pod crashes or deleted a new pod will automatically created.    
3  **Node Failure Recover**  
  . If pod goes down a qubescheduler will shedule a new nodes.    
4. **Horizontal pod Autoscaler (HPA)**  
  . Not exactly healing, but it can automatically scale pod up/Down to avoid overload failures.  

  ### Example Deployment with Auto-Healing  
  ```yaml
  apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
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
```
💡 Quick Interview Answer:  
 
 "K8s auto-heals workloads by restarting unhealthy containers using liveness probes, rescheduling pods to healthy nodes if a node fails and maintaining the desired replica count via **ReplicaSets**. This ensures minimal downtime without manual intervention."

## Question :  
### What are key factors to secure kubernetes cluster.

1 **API Server & Access Control**  
 . Enable RBAC   
 . use AzureAD INtegration for AKS authentication.  
 . Restrict API server access with authorized IP ranges.    
  
2 **Network Security**  
 . Implement Network policies to control pod-to-pod and pod to external traffic.  
 . Use Azure NSG & firewall for cluster  
 . Disable public access to sensitive services.  
3 **Pod Security**  
 . Enforce Pod Security Standards. OPA/Gatekeeper to prevent running privileged containers.  
 . Run containers as non-rot users.    
 . Limit conatiner capabilities.  
 4 **Secrets Management**  
  . Store secrets in azure key-vault, not plain-text in manifests.  
  . Enable kubernetes Secrets encryption at rest.  
 5 **Cluster Maintenance**  
  . Regular patch and updates k8s version & node OS.  
  . Use Azure Defender for k8s for runtime threat detection.    
  6 **Image Security**  
  . Use only trusted images from private registry(ACR).    
  . Scan images for vulnerabilities with Microsoft Defender.  

💡 Quick 15-sec answer for interviews:

“I secure AKS clusters by enforcing RBAC, network policies, and pod security standards, integrating with Azure AD, storing secrets in Key Vault, scanning images for vulnerabilities, and enabling monitoring and threat detection with Azure Defender.”
