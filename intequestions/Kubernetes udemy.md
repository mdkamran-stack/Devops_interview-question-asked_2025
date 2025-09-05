

Etcd Version  

## create a pod with the name redis and with the image redis123  

kubectl run redis --image=redis123 --dry-run=client -o yaml  

kubectl run redis --image=redis123 --dry-run=client -o yaml >redis.yaml  
kubectl create -f redis.yaml  
kubectl get pods  

## Now change the image on this pod to redis.

vi redis.yaml >> chnage the image name to redis  
kubectl apply -f redis.yaml  










