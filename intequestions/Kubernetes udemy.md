

Etcd Version  

## create a pod with the name redis and with the image redis123  

kubectl run redis --image=redis123 --dry-run=client -o yaml  

kubectl run redis --image=redis123 --dry-run=client -o yaml >redis.yaml  
kubectl create -f redis.yaml  
kubectl get pods  

## Now change the image on this pod to redis.

vi redis.yaml >> chnage the image name to redis  
kubectl apply -f redis.yaml  

## Replicaset 
kubectl get replicaset 
kube get pods 

To scale replicas there is 2 ways 1st update in template file 2nd via cmd line  
kube scale --replicas=2 -f replicaset-defination.yaml  
kube scake --replicas=2 replicaset myapp-replicaset   >> replicaset is tye  & my-replicaset is name  
kube create -f replicaset-defination.yaml    >> to create replicaset  
kube replace -f replicaset-defination.yaml   >> for replace
kubectl get rs -n default --no-headers | wc -l >> to get no.of replicaset  

# Kubernetes: NGINX Pod & Deployment

## Create an NGINX Pod
```bash
kubectl run nginx --image=nginx
Generate POD Manifest YAML file (Don’t create, use --dry-run)
bash
Copy code
kubectl run nginx --image=nginx --dry-run=client -o yaml
Create a Deployment
bash
Copy code
kubectl create deployment --image=nginx nginx
Generate Deployment YAML file (Don’t create, use --dry-run)
bash
Copy code
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
Generate Deployment YAML file and Save to a File
bash
Copy code
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
Make necessary changes to the file (e.g., add more replicas) and then create the deployment:

bash
Copy code
kubectl create -f nginx-deployment.yaml
Alternative (K8s v1.19+): Create Deployment with Replicas in One Command
bash
Copy code
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yam
```

## LAB service:
How many services exist in the system? in the current namesapce   kube get svc

What is the atrgetport configure on the k8s service?  kube describe svc nameof service

What is the image used to create the pods in the deployment?
kube get deploy && kube describe deploy deployname

## To create a service we can refer k8s documentations

vi cretae the service & then kubectl create -f service.yaml

## NAme space

Create a POD in the finance namespace.   kubectl run redis --image=redis -n finance  

Which namespace has the blue pod in it?   kubectl get pods --all-namespaces   


## LAB 49
Create a service named redis-service to expose the existing redis pod within the cluster on port 6379.  

 kubectl expose pod redis --port=6379 --name=redis-service --type=ClusterIP  

 


