# LLyods bank asked questions.

## Deployment vs demonset?

"**Deployment**" manages the desired state of pods and handles rolling updates, "**DaemonSet**" runs a pod on every node or specific nodes in the cluster.

## What is Kubernetes?
👉 “An orchestration platform for automating container deployment and scaling.”

## what is cluster
A cluster is the collection of nodes where Kubernetes runs apps.
## What is a Pod?
👉 “The smallest deployable unit, running one or more containers.”

## What is the role of etcd in Kubernetes?

etcd is Kubernetes’ key-value store that persistently stores all cluster state and configuration data

## How do Ingress and Ingress controllers work?
Ingress defines the routing rules for external traffic, while an Ingress Controller enforces those rules by configuring a reverse proxy or load balancer to route traffic into the cluster.

## What is the difference between Ingress and a LoadBalancer Service?

LoadBalancer exposes one Service; Ingress routes to many Services.

## How would you secure communication between Pods in Kubernetes?
I secure Pod-to-Pod communication using NetworkPolicies to restrict traffic, mTLS for encryption/authentication, and RBAC with Secrets to control access to sensitive data

##  How Pod autoscale.

Pod autoscaling in Kubernetes is done using the Horizontal Pod Autoscaler (HPA), which scales Pods based on CPU, memory, or custom metrics.

## How do you upgrade a Kubernetes cluster with minimal downtime?

“To upgrade a Kubernetes cluster with minimal downtime, I perform a rolling upgrade of control plane components first, then upgrade worker nodes one by one, ensuring workloads remain available throughout the process.

## What is a PersistentVolume (PV) and PersistentVolumeClaim (PVC)?

A PersistentVolume (PV) is a cluster resource representing storage, and a PersistentVolumeClaim (PVC) is a user request for storage,

## How do you handle zero-downtime deployments?

I handle zero-downtime deployments in Kubernetes using rolling updates, blue-green, or canary deployments, ensuring old Pods remain available until new ones are ready and traffic is safely switched.

## What is RBAC (Role-Based Access Control) in Kubernetes?
RBAC in Kubernetes controls access to resources by assigning roles to users or service accounts, defining what actions they can perform on which resources.
## Can you explain what is virtualization and containerization? Why do we need containerization?

Virtualization runs full VMs with separate OS, while containerization packages apps with dependencies in lightweight, portable containers that are faster, scalable, and consistent across environments

## Kubernetes architectures.

Every k8s cluster has Master and worker node.  

Master node having 4 components.     **"1 API server**"    **"2 ETCD**"     **"3 scheduler**"    **"4 control manager**"  
Worker Node having 3 components      **"1 Kubeproxy**"           **"2 Kubelet**"        **"3 container run time**"  

API server : Authenticate the request and get the data Eg: kubectl get pods run in background.  

ETCD: is brain of k8s cluster stores all meta data of all the resources.

Scheduler : Schedule your pod on node based on CPU & memory requiremets that we have specify.  

Control manager : Manages all the diffrent controller like replication controoler deployment controller job controller & node controller.  

Kubelet : it is reponsible to communicate state of pod running on node back to API server.  

Kubeproxy: is responsible for intercommunication of pod.  

CRT: is nothing but container run time S/W like docker crio containerd.  


  ## How are you fetching the secrets that are required inside the Kubernetes cluster?  

 We usually fetch secrets through external secret managers like AWS Secrets Manager The operator syncs the external secrets into Kubernetes Secrets, which my pods consume via environment variables or mounted volumes. so pods can securely fetch secrets directly without hardcoding credentials.”

## How does Service discovery work in Kubernetes?
Kubernetes uses DNS to map Service names to stable IPs
 ## How are you going to fetch it from the application?  

 The application fetches secrets through environment variables or mounted files from Kubernetes Secrets, which are synced from external secret managers if needed.

 ## There is something called Kubernetes upgrade control plane and other things. What is that difference?

 “In Kubernetes, the control plane manages the cluster components like the API server, scheduler, controller manager, and etcd. Upgrading the control plane updates these master components. Worker node upgrades, on the other hand, update the nodes where workloads run. So ‘control plane upgrade’ affects cluster management, while ‘node upgrade’ affects the application runtime environmen.  

 ## What is difference between deployment and statefulset in kubernetes?

 Deployment is for stateless apps with identical pods, while StatefulSet is for stateful apps that need stable identity, ordered scaling, and persistent storage.

 ## what is stateful and stateless

Stateless apps don’t retain data between requests and are easy to scale, while stateful apps maintain data/session and need persistence with stable identity.

 ## What are the things you'll consider before doing the upgrade?

 Before upgrading Kubernetes, I consider compatibility of workloads and add-ons, backup of etcd and cluster state, version support (control plane vs nodes), testing in a staging environment, maintenance windows to avoid downtime, and a rollback plan in case the upgrade fails.  

 ## Where are you going to check these things?

 check these things in the official Kubernetes release notes for version compatibility, cloud provider documentation (like AWS EKS or Azure AKS) for supported upgrade paths, cluster add-on versions (like CNI, CoreDNS), and existing workloads using kubectl and monitoring dashboards to ensure readiness.  

 ## How does kubelet handle node pressure? What happens when the disk fills?

 “Kubelet handles node pressure by monitoring resource thresholds and evicting low-priority pods when disk, memory, or inode usage gets critical—starting with cleanup and escalating to eviction if needed.

##  You have 160 applications which are there inside the cluster. You cannot go for each deployment to check the logs or to describe. What is that one place you will check config secrets and other volume mounts and other things? How many applications are there in your cluster?

For many applications, I check centralized resources like kubectl get all --all-namespaces, secrets, config maps, and volumes, or use dashboards like Lens/Octant to get an overview, and count applications via kubectl get deployments --all-namespaces.  

## What are the types of volume mounts you have and what is the difference?

Volumes differ by purpose: EmptyDir is ephemeral, HostPath is node-specific, PV/PVC is persistent, ConfigMap/Secret store configs, Projected combines sources, and CSI integrates external storage.  

## What is the difference between persistent volumes and ephemeral volumes?

Persistent volumes outlive Pods for durable storage, while ephemeral volumes exist only for a Pod’s lifetime for temporary data.  

## How will you integrate it with the cluster so that Grafana will be able to fetch the logs and it will show in the dashboards?

Prometheus scrapes cluster metrics, Grafana is configured as its data source, and dashboards visualize metrics; for logs, tools like Loki can be integrated similarly.

 ## Can you describe a time when you had to troubleshoot a production issue and how you resolved it?

 **Situation:**

“In one of my previous projects, a critical production service started experiencing intermittent outages during peak traffic hours. Users were reporting slow responses and timeouts.”

**Task:**

“As the on-call DevOps engineer, my task was to quickly identify the root cause, restore service stability, and prevent recurrence.”

**Action:**

“I checked application and system logs, then used Kubernetes kubectl describe and monitoring dashboards (Prometheus & Grafana) to analyze metrics. I discovered that Pods were hitting CPU limits, causing them to restart frequently. To resolve it, I temporarily scaled the replicas to handle the spike and increased the resource limits. Later, I worked with the team to optimize the application code and set up Kubernetes Horizontal Pod Autoscaler so scaling became automatic.”

## How to Troubleshoot a Failed Prod Deployment in Kubernetes

To troubleshoot a failed production deployment in Kubernetes, I start by checking the Deployment and Pod status with kubectl describe. Then I review Pod logs, events, and resource usage to identify errors like image pull failures, CrashLoopBackOff, or config issues. I also check networking and dependencies. If the issue is critical, I roll back using kubectl rollout undo. This systematic approach helps quickly isolate and fix the problem while minimizing downtime

## How do you handle Kubernetes pod scheduling failures?  

Answer: Run kubectl describe pod → Check taints/tolerations → Check node resources → Add tolerations or scale nodes. 

## How do you enforce least privilege in Kubernetes?  

Answer: Use RBAC roles → Bind only necessary permissions → Restrict cluster admin → Enable PodSecurityPolicies/OPA. 

## Prometheus and grafana installations what is difference? 

Prometheus Prometheus is a time-series monitoring and alerting system, while Grafana is a visualization and dashboarding tool that can use Prometheus (and others) as a data source.

**Architecture of prometheus**
Prometheus scrapes metrics from target server, stores them in a time-series DB, queries with PromQL, integrates with Grafana for dashboards, and uses Alertmanager for notifications.”


## ques 4 : how did you check Health of conatiner configuration in k8s.  

I check container health in Kubernetes using liveness, readiness, and startup probes defined in the Pod spec and verify their status with kubectl describe pod and logs.

### How it works  
. LivenessProbe -> if /health fails repetadely, pod is restarting.   
. readinessProbe -> if /ready  fails, pod stays running but is removed the service load balancer.   
. Startup Probe -> (Optional) Prevents liveness from killing slow-starting apps.   

## Difference between secrets vs configmap.

A ConfigMap stores non-sensitive configuration data in plain text, while a Secret is meant for sensitive data (like passwords, tokens, certs) and is base64-encoded (and can be encrypted at rest)

## Ques: In Kubernetes, How the auto-healing mechanism automatically detects and recovers from failed workloads without manual intervention.  

### How Auto Healing works.  
Kubernetes has a built-in self-healing mechanism. It continuously monitors Pods using controllers and health probes. If a Pod crashes, fails a liveness check, or a node goes down, Kubernetes automatically restarts the container or reschedules the Pod on a healthy node to maintain the desired state. This happens without any manual intervention, ensuring application availability and resilience

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
  . Use only trusted images from private registry(ECR).      

💡 Quick 15-sec answer for interviews:

“I secure EKS clusters by enforcing Kubernetes RBAC and IAM roles for service accounts (IRSA), implementing network policies, and applying Pod Security Standards. I integrate authentication using AWS IAM and IAM Identity Center, store secrets securely in AWS Secrets Manager, scan container images in Amazon ECR using Amazon Inspector, and enable monitoring and threat detection with Amazon CloudWatch, GuardDuty, and Security Hub.”

 ## Prod Kubernetes cluster is unstable — pods aren’t pulling images, some are evicted. What’s your approach?

Start by inspecting pod status using kubectl describe pod. If images aren’t pulling, check image name, tag, and registry permissions. For evicted pods, check node pressure (disk/memory) with kubectl describe node. Prevent issues by enforcing resource limits, setting up monitoring, and implementing PodDisruptionBudgets.

## How do you iplement network policyto restrict pod-to-pod communication in Kubernetes?

Use Kubernetes NetworkPolicies. These define rules based on pod selectors, namespaces, and ports. Ensure your cluster uses a network plugin that supports them, such as Calico

## How would you do blue-green deployments in Kubernetes?

Deploy two versions of the application (blue and green) and switch traffic between them using a Kubernetes Service. You can use Ingress or service label selectors to change which pods receive traffic. For gradual rollouts, tools like Argo Rollouts or Istio are recommended.

## What are the different services we have in Kubernetes  

## ClusterIP (default)

Exposes the service internally within the cluster.

Use case: Pod-to-Pod communication.

## NodePort

Exposes the service on a static port on each node.

External traffic can access the service via NodeIP:NodePort.

## LoadBalancer

Provisioned when using a cloud provider.

Exposes the service externally through a cloud load balancer.

## ExternalName

Maps the service to a DNS name external to the cluster.

Useful for connecting to external services.

## Headless Service

Service without a cluster IP (clusterIP: None).

Often used with StatefulSets for direct Pod communication
