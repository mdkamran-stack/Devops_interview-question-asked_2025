# This commit is for questions asked me as devops interview in 2025

# Pepsico
==========
### What is output of Maven Build, if it is jar file or war file what command will put in docker file?
### scenario: you have one repo and want to build a pipeline to deploy to AWS (e.g., ECS, EKS, 
### how is your application is connected in frontend in your cluster 
### authentication and autherization who will manage application or loadbalancer ? 
### how it will authenticate weather user is valid or not  
### what is HIGH IO usage  
### how to export metrics to prometeus? 
###  lets say you have application java running micro services in kubernetes cluster from there to grafana
### 3 instance in 3 diff regions 
### 3 ec3 instance load from external source i have to divide traffic equally on all 3 
### git stash 
### shared lib in jenkins 

# Bristlecone interview
------------------------
### create 10 ubnut server with apache installations  /var/lib/html via terraform   
### write shell script get the process with user name kamran and kill the process   
### how to deploy application in k8s     
### how to setup k8s in eks    
### types of loadbalancer in k8s    
### how to setup hpa na vpa    
### what is selinux    
### what is loadbalncer in aws & how to setup app LB and NW LB , how network LB works    
### What is the difference between the count and for_each meta-arguments?    
### how to communicate with two infrastructure?  
### how to migrate on-perm to cloud?   
### how to communicate microservices?  

# QuessCorp
------------------
### Terraform list vs terraform show  
### IN CI/CD pipeline how to configure that pipeline run during change window and deploy app in cluster  
### In prometheus grafana lets say we have distributed monitoring lsts say resources are spread across diff region how we can build now.  
### In terraform dependecy error like linear & circular  
### What is lifecycle in TF  

# TechMahindra
---------------
### Suppose we have EKS cluster version X to upgrade upper version 3 node cluster while ensuring zero downtime? how did you do that?
### How did you upgrade the kubernetes cluster on zero downtime.
### Jenkins CI fails how to fix 
### Write a jenkins pipeline trigger from main branch Build & push to ACR before that in ci pipeline we need code scan onc done trigger to cd pipeline?  
### Azure pipeline taking 40 minutes in ci stage randomy fails in test stage how did you troubleshoot?  
### How did you integrate terraform to azure ci cd pipeline?  
### If someone manully create in cloud how to fix in term of terraform?
### You are in call getting crashloopback error.  
### Write a terraform file for first create one resource group on top of it create resource group, vnet , 2 subnet , private endpoint?  
### Difference bw nodeselector and taint and toleration?  
### Write a yaml for ingress pod name space should my namespace?  
### why we are using ingress into AKS?  
### what is pv and pvc ?  
### we have nodeselector in particular pod going to deploy new pod without objection on particular nodespace so it cant restrict that pod  
### i want build a new nodepool for our aks cluster i want version memory disk pace OS version network taint and toleration how to achive using yaml file.  
### In AKS we have 3-4 pipeline dev uat test n prod once development done go for next step prd time we have approval gateway and deploy to prod.  
### what is kubeconfig 
#### we have deploy 3 node cluster how many deploys 
master node build that 

# Github workflows & yaml exp migrating azure to githubaction flows , aks deployment .most deploy in aks.

# Photon
==========
### Questions: A jenkins pipeline takes 45 mins to build microservices Build how do you reduces it less tah 10 minutes without adding more agents.   
### Questions: Developers complains there is pipeline is getting failed no actionable log what do you change.
### Questions: A microservices in kubernetes become slow during peak hours no code chnage what do you investigate?  
### Questions: Elastic search cluster storage cost is expoding what is your plan?  
### Question: Kafka consumer log grows during spike how do you troubleshoot it.  
### Question: Production hotfix must go urgenty but multiple teams have ongoing change in developed branch?   
### Questions: CPU spike for a workload but no alert is triggered how do you fix monitoring?  
### Question: A service itermittently become unreachable how do you troubleshoot?   
### Question : JAVA 8 service must upgrade to JAVA17 how do you scale large migration.  
### Question: A critical incident happen at night you are esclation point what your action plan?  
### Question: How do you design a zero downtime multi region deployment?   
### Question: A deployment to production fail half way some pods are running older version some in new version what do you do?   
### Question: Jenkins pipeline is frequently fails due to intermitent network issue while pulling dependencies what your approach?   
### Question: Kafka topics retention eating up disk what your quick fix?    
### Question: your cluster root certificate expire in next 5 days what is your plan?   
### Question: your jenkins pipeline takes 20 mins before even starting unit test becasuse its download dependecies every time how do you fix it without modifying developer machine?  
### Question: Latency spike occured during high load metrics show cpu is fine but conetxt switching is high?  
### Question: A pod JVM service uses 80% cpu deposite load traffic what do you inspect?  
### QuestionA Sudden spike in 400 error occur what you check first as devops perspective?  
### Question You must provide a audit log for all infrastructure changes what solution do you implement 
### Question A micro services deployment causes API Gateway 502 error even through pods are healthy what your approach
-----------------------------
# AHEAD questions asked me for AWS cloud devops Engineer
---------------------------------------------------------------------
### Question: Recent infrastructure architecture in aws  >> see in aws file infra provisioning project.
### Question: Expalin vpc component and traffic flow how it happen  
### Question: How does NAT Gateway works internally  
### Question: diff b/w security group vs nacl  
### Question: How to create  highly availabe web application in aws  
### Question:  Diff b/w ALB VS NLB when do you choose 
### Question: Explain me CI/CD pipeline which you have build in AWS  
### Question: EC2 based application is slow during high traffic cpu is normal response time is high how to troubleshoot.   
### Question: Production application behind ALB app LB return 502 error intermitently how to troubleshoot.
### Question: Your AWS bill jump from 1k to 4k in a month how do you identify the root cause and prevent for next time recurrence.  
### Question: we have 50 developers need aws access no shared access allowed how to achive using tf  
### Question: how do you enable zero downtime deployment

Get thses question from whatapp resources.
========================
1. Production AKS app went down at 2 AM. What is your immediate action plan?
 2. Pods are in CrashLoopBackOff after deployment. How will you troubleshoot?
 3. Deployment works in Dev but fails in Prod. What will you compare first?
 4. Terraform apply failed midway and some Azure resources are created. What next?
 5. Someone modified Azure resource manually which is managed by Terraform. How do you handle drift?
 6. AKS cannot pull image from ACR. How will you debug?
 7. Production pods CPU is 90 percent. What steps will you take?
 8. AKS node shows NotReady. What is your investigation approach?
 9. How will you design zero downtime deployment for critical application?
 10. Azure DevOps pipeline is successful but changes not reflecting in AKS. Where will you check?
 11. How will you structure Terraform for Dev, QA, and Prod environments?
 12. How do you secure secrets in Kubernetes for production workloads?
 13. Cluster autoscaler is not scaling nodes even when CPU is high. What will you verify?
 14. Terraform state file accidentally deleted. What are recovery steps?
 15. How will you configure private AKS cluster with restricted access?
 16. HPA configured but pods are not scaling. What could be wrong?
 17. Production deployment failed and business wants immediate rollback. What is your approach?
 18. How will you migrate manually created Azure infrastructure to Terraform?
 19. CI/CD pipeline taking 40 minutes. How will you optimize it?
 20. Multiple engineers working on same Terraform code. How do you prevent conflicts?
 21. How will you expose AKS app using Application Gateway instead of LoadBalancer?
 22. Ingress configured but traffic not reaching pods. What will you check?
 23. Node showing disk pressure error. How will you fix?
 24. How will you restrict production access only to specific IP addresses?
 25. Application needs persistent storage in AKS. What will you use and why?
 26. After new release, some users still see old version. What could be the issue?
 27. How will you implement Blue Green deployment in AKS?
 28. Security team says AKS cluster is publicly accessible. How will you secure it?
 29. How will you rotate secrets without downtime?
 30. There is complete production outage. How do you handle troubleshooting and stakeholder communication?




