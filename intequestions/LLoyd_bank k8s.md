# LLyods bank asked questions.

##  How Pod autoscale.

Pod autoscaling in Kubernetes is done using the Horizontal Pod Autoscaler (HPA), which scales Pods based on CPU, memory, or custom metrics.

## Kubernetes architectures.

Every k8s cluster has Master and worker node.  

Master node having 4 components.     1 API server          2 ETCD          3 scheduler        4 control manager  
Worker Node having 3 components      1 Kubeproxy           2 Kubelet        3 container run time  

API server : Authenticate the request and get the data Eg: kubectl get pods run in background.  

ETCD: is brain of k8s cluster stores all meta data of all the resources.

Scheduler : Schedule your pod on node based on CPU & memory requiremets that we have specify.  

Control manager : Manages all the diffrent controller like replication controoler deployment controller job controller & node controller.  

Kubelet : it is reponsible to communicate state of pod running on node back to API server.  

Kubeproxy: is responsible for intercommunication of pod.  

CRT: is nothing but container run time S/W like docker crio containerd.  

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

## How to Troubleshoot a Failed Prod Deployment in Kubernetes

To troubleshoot a failed prod deployment in Kubernetes, check rollout status, pod/events, logs, configs/secrets, services/networking, node resources, and roll back if needed.” ✅

You can also keep a quick stepwise checklist in mind:

kubectl get deployments -n <namespace> → check rollout status

kubectl get pods -n <namespace> → check Pod states

kubectl describe pod <pod-name> -n <namespace> → check events

kubectl logs <pod-name> → inspect logs

Verify ConfigMaps & Secrets

Check Services, Ingress, networking

Check node resources (kubectl describe node)

Roll back if needed (kubectl rollout undo deployment/<deployment-name>)

## How do you handle Kubernetes pod scheduling failures?  

Answer: Run kubectl describe pod → Check taints/tolerations → Check node resources → Add tolerations or scale nodes. 

## How do you enforce least privilege in Kubernetes?  

Answer: Use RBAC roles → Bind only necessary permissions → Restrict cluster admin → Enable PodSecurityPolicies/OPA. 

## ques 4 : how did you check Health of conatiner configuration in k8s.  

I check container health in Kubernetes using liveness, readiness, and startup probes defined in the Pod spec.”


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

## Difference between secrets vs configmap.

A ConfigMap stores non-sensitive configuration data in plain text, while a Secret is meant for sensitive data (like passwords, tokens, certs) and is base64-encoded (and can be encrypted at rest)

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

 ## Prod Kubernetes cluster is unstable — pods aren’t pulling images, some are evicted. What’s your approach?

Start by inspecting pod status using kubectl describe pod. If images aren’t pulling, check image name, tag, and registry permissions. For evicted pods, check node pressure (disk/memory) with kubectl describe node. Prevent issues by enforcing resource limits, setting up monitoring, and implementing PodDisruptionBudgets.

## How do you control pod-to-pod communication in Kubernetes?

Use Kubernetes NetworkPolicies. These define rules based on pod selectors, namespaces, and ports. Ensure your cluster uses a network plugin that supports them, such as Calico

## How would you do blue-green deployments in Kubernetes?

Deploy two versions of the application (blue and green) and switch traffic between them using a Kubernetes Service. You can use Ingress or service label selectors to change which pods receive traffic. For gradual rollouts, tools like Argo Rollouts or Istio are recommended.
