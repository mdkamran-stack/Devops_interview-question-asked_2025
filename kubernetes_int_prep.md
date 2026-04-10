## Deployment vs demonset?

"**Deployment**" manages the desired state of pods and handles rolling updates, "**DaemonSet**" runs a pod on every node or specific nodes in the cluster.

## What is Kubernetes?
👉 “An orchestration platform for automating container deployment and scaling.”

## taint & toleration

A taint is applied to a node to prevent pods from being scheduled on that node unless they tolerate it.

A toleration is applied to a pod so it can run on nodes with taints.

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

## How do you upgrade a Kubernetes cluster with minimal downtime?

“To upgrade a Kubernetes cluster with minimal downtime, I perform a rolling upgrade of control plane components first, then upgrade worker nodes one by one, ensuring workloads remain available throughout the process.

## What is a PersistentVolume (PV) and PersistentVolumeClaim (PVC)?

A PersistentVolume (PV) is a cluster resource representing storage, and a PersistentVolumeClaim (PVC) is a user request for storage,

## Types of scaling in k8s?

Horizontal Pod Autoscaler scales the number of pods based on metrics like CPU or memory usage to handle traffic load. Vertical Pod Autoscaler adjusts the CPU and memory resources assigned to a pod based on its usage. HPA is mainly used for scaling applications under high traffic, while VPA is used for optimizing resource allocation.
 
1 Recommendation Mode : Provide resource recommendations without applying changes.
2 Auto Mode: Automatically updates resource requests & restart pods.  
3 Initial Mode Applies recommended resources when pods are first created.

### cluster scaling adjust the no of nodes in kubernetes cluster , when cluster dont have enough capacity to schedule pods new nodes are automatically added.  

### Multi-cluster scaling used for large system . eg : Improved fault tolerance , Geographical distibution , independent scaling.

## How to upgrade EKS cluster?
1 Prerequisutes Cordon Nodes  unscheduable during this process of k8s cluster upgrade during this process takes (1-2 ) hours but customer cant impacted for 1-2 hours we stop any new deployment .  
2 Go through Release Notes for what changes in new release there can be change in particular feature works.  
3 Strart with lower ENV Eg: dev we cant downgrade from upper version to lower version its best practice to upgrade lower env first and wait for a week.   
4 control plane as well as Nodes should be in same version (suppose our control plane is 1.30 and nodes is 1.29 then we have to first upgrade the node to 1.30 version)   
5 kubelet and cluster autoscaler are compatible with control plane     
6 we need  5 available ip address  in subnet   
Uprade the control plane first & then upgrade nde group   
uprades addons Eg: kube proxy vpc CNI   
make sure everything works on lower env so that it works on higher ENV.  we used Rollout process for upgradation rollout basically upgrades one by one nodes   
After upgradation QE team will proceed with functional testing.    

## How do you handle zero-downtime deployments?

I handle zero-downtime deployments in Kubernetes using rolling updates, blue-green, or canary deployments, ensuring old Pods remain available until new ones are ready and traffic is safely switched.

## How do you find out what environment variables a running pod has?"

kubectl exec <pod> -- env — or kubectl describe pod to see the sources (secretRef, configMapRef).

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
Liveness probe checks whether the application inside a container is still running. If it fails, Kubernetes restarts the container automatically. Readiness probe checks if the application is ready to accept traffic. If it fails, the pod is removed from the service endpoints but the container is not restarted.   
. Startup Probe -> (Optional) Prevents liveness from killing slow-starting apps.   

## Difference between secrets vs configmap.

A ConfigMap stores non-sensitive configuration data in plain text, while a Secret is meant for sensitive data (like passwords, tokens, certs) and is base64-encoded (and can be encrypted at rest)

## Ques: In Kubernetes, How the auto-healing mechanism automatically detects and recovers from failed workloads without manual intervention.  

## Interview Question: "What is ArgoCD and how does it implement GitOps?"

ArgoCD is a declarative GitOps tool for Kubernetes. It continuously polls Git repositories and compares the desired state (YAML/Helm in Git) with the actual state in the cluster. When drift is detected, it either alerts (manual sync) or automatically reconciles (automated sync with selfHeal). Every deployment is a Git commit — full audit trail, easy rollback, no manual kubectl in production.

## Interview Question: "What is the difference between ArgoCD Sync and Health status?"

Sync status: does the cluster match what's in Git? (Synced / OutOfSync) Health status: is the application actually running correctly? (Healthy / Degraded / Progressing) A deployment can be Synced but Degraded — e.g., ArgoCD applied the manifest but pods are CrashLooping.

## Interview Question: "What is selfHeal in ArgoCD?"

When selfHeal: true, ArgoCD automatically reverts any manual changes to the cluster back to the Git state. This enforces GitOps strictly — the cluster always matches Git, even if someone runs kubectl directly.

## Interview Question: "What is Helm and why use it over plain YAML?"

Helm is a Kubernetes package manager. A Helm chart is a collection of templated YAML files — values are injected at deploy time from a values.yaml file. This means one chart serves all environments (dev/qa/prod) — you only change the values. In our project, one shared helm-charts/ folder deploys all 4 backend services, each with its own values file. Without Helm you'd have 20 nearly-identical YAML files to maintain.

## Interview Question: "What is the difference between helm install and helm upgrade --install?"

helm install fails if the release already exists. helm upgrade --install creates it if missing or upgrades it if it exists — idempotent, safe to run repeatedly. Always use helm upgrade --install in CI/CD pipelines.  

## Interview Question: "What is GitOps and why is it better than running kubectl apply manually?"

GitOps uses Git as the single source of truth for infrastructure state. ArgoCD continuously reconciles the cluster state with what's in Git. Benefits: full audit trail (Git history), easy rollback (git revert), no manual kubectl in production, drift detection (if someone changes something manually, ArgoCD detects and reverts it).

## Interview Question: "How do you debug a microservice that is returning 500 errors?"

First kubectl get pods -n dev to check pod health and restart count. Then kubectl logs <pod> to read the application logs — look for exceptions. kubectl describe pod <pod> to check events (OOMKill, probe failures, image pull errors). If the pod is running, kubectl exec -it <pod> -- /bin/sh to inspect the environment and test connectivity to dependencies.

## Interview Question: "What is RBAC in Kubernetes and why does it matter?"

RBAC controls what actions each identity (user, service account) can perform on which resources. A Role defines permissions within a namespace; a ClusterRole applies cluster-wide. In our project, each microservice has its own ServiceAccount — the api-gateway's ServiceAccount has an IAM role annotation (IRSA) that allows it to access AWS services. This follows least-privilege: each pod only has the AWS permissions it actually needs.

## Interview Question: "How does ArgoCD use Helm?"

ArgoCD has Helm built in. When an Application has path: helm-charts with helm.valueFiles, ArgoCD runs helm template locally to generate the final YAML manifests, then applies them. It doesn't use helm install — it uses Helm purely as a template engine, then manages the resources itself.

## Interview Question: "How does HPA decide when to scale?"

It checks the metrics-server every 15 seconds. If average CPU across all pods exceeds the target percentage, it adds pods (up to maxReplicas). When load drops, it scales down after a cooldown period (default 5 minutes). 

  ## Concept: Kubernetes uses probes to know if a pod is alive and ready to serve traffic.  

 livenessProbe — if this fails 3 times, Kubernetes restarts the pod
readinessProbe — if this fails, Kubernetes stops sending traffic to the pod (but doesn't restart it)
initialDelaySeconds: 60 — Spring Boot takes ~60s to start, so Kubernetes waits before checking  

## How does Kubernetes achieve zero-downtime deployments?"

Rolling update strategy. maxSurge: 1 means it can create 1 extra pod above desired count. maxUnavailable: 0 means no pod is removed until the new one is Ready. So at no point is capacity reduced below 100%. Rolling update is a zero-downtime deployment strategy where new application versions are deployed gradually by replacing old instances one by one.

## Interview Question: "What is the difference between liveness and readiness probes?"

Liveness: is the app alive? If it fails, restart the pod. Readiness: is the app ready for traffic? If it fails, remove from Service endpoints but don't restart. Use readiness to handle slow startups or temporary overloads without killing the pod.  

## Interview Question: "How does the Kubernetes scheduler decide where to place a pod?"

It filters nodes that meet the pod's requirements (enough CPU/memory requests, matching nodeSelector/affinity rules, no taints). Then it scores the remaining nodes — preferring nodes with more available resources, spreading replicas across nodes (if anti-affinity is set). In our prod values, we use podAntiAffinity to ensure two replicas of the same service never land on the same node.

# Interview Question: "How do microservices discover and communicate with each other in Kubernetes?"

Via Kubernetes DNS. Every Service gets a DNS name: <service>.<namespace>.svc.cluster.local. In our project, the api-gateway is configured with AUTH_SERVICE_URL=http://auth-service:8081 — Kubernetes DNS resolves auth-service to the ClusterIP of the auth-service Service, which load-balances across all auth-service pods.

## Ingress — Change Routing Rules
Concept: The Ingress Controller routes traffic based on host and path rules.  

## Interview Question: "How does nginx ingress route traffic to different services?"

nginx ingress controller watches for Ingress resources and generates nginx configuration automatically. When a request arrives, nginx checks the Host header and path against the rules. In our project, path /api goes to api-gateway and / goes to pharma-ui — both share a single ELB entry point.

## Interview Question: "What is the difference between resource requests and limits?"

Requests: what the pod is guaranteed — used by the scheduler to find a node with enough capacity. Limits: the maximum a pod can use — if it exceeds memory limit, the kernel OOMKills it. If it exceeds CPU limit, it is throttled (slowed down, not killed).

## Interview Question: "What is the difference between a ConfigMap and a Secret?"

Both inject config into pods as env vars or files. ConfigMaps are for non-sensitive config (log level, port, URLs). Secrets are for sensitive data (passwords, tokens) — stored base64-encoded in etcd, and in our project, synced from AWS Secrets Manager by External Secrets Operator so they never touch Git.

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
