

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


















