## Diff b/w copy vs add ?

Copy is used to copy a file or dicrectory from host machine to docker image.  
ADD instructions i used for extracting tar ball and copying remote url  
Recommedned copy  

## what is persistent volume in docker.

1 volume wheb we use volume option it will create new volume in /var/lib/docker directory and data will be saved in remote machine f we removed container we can attach one volume to multiple conatiner
2 Bind mount a file & dir from a host machine is mounted into a container.

## To commnicate two conatiner as ip is not static 
We can use container name it is always static 

## What is docker file & its instructions.
Docker file is text file which uses intruction to assemble docker images, we can custom acc to our need.  

1 FROM: docker file is starts with from it is first inst whic create base image layer, after that each inst is create further layer.  
2 COPy : is used to copy afile from host machine to docker image.  
3 RUN : it is used to run cmd inside the image.  
4 ENV: it sets the ENV varible that can be used inside the docker file.  
5 CMD : it specify what command start when conatiner starts, just like cmd we can use ENTRY point to do same job.  
 
WORKDIR  It set current working directory permanently   

## ENRTPOINT VS CMD: CMD when you want a default command that users can override. Use ENTRYPOINT when you want a fixed command, but user can append it during runtime.
# CMD 
## diff bw command mode vs shell mode
Shell mode runs commands via a shell and supports shell features, while command (exec) mode runs commands directly and is preferred for production due to better signal handling.

Shell Mode (Shell Form)  CMD echo "Hello World"

Command Mode (Exec Form) CMD ["echo", "Hello World"]  

## Can you write a real-world multi-stage Dockerfile for a Node.js app?

 Stage 1: build  

FROM node:18   
WORKDIR /app  
COPY package*.json ./  
RUN npm ci  

Stage 2 : production  
FROM node:18-slim  
WORKDIR /app    
COPY --from=build /app/dist ./dist  
COPY package*.json ./  
RUN npm ci--only=production  
CMD ["node", "dist/index.js"]    

This separates the build and runtime environments, reducing image size and surface area

## One container needs to communicate with another container. How would you do it?

Containers communicate over a shared network using service discovery—via container names in Docker or Services and DNS in Kubernetes—rather than using localhost or fixed IPs.

## A container crashed. How do you figure out what went wrong?

Use docker logs <container_id> to view logs. Run docker inspect to get the exit code and details. Check for memory limits, incorrect entrypoints, missing files, or permission issues. In Kubernetes, also use kubectl describe pod and kubectl logs.

## How do you reduce Docker image size?
I reduce Docker image size by using minimal base images, multi-stage builds, cleaning dependencies, minimizing layers, and excluding unnecessary files with .dockerignore

## How do you troubleshoot failed Docker image push to registry? 
Answer: Check registry credentials → Validate image name/tag → Ensure repository exists → Retry with correct login. 

## Have you deployed any application to EKS? If yes, how?

1️⃣ Create EKS cluster    2️⃣ Configure access    3️⃣ Build Docker image & push to ECR  4️⃣ Create Kubernetes manifests || Deployment || Service   5️⃣ Deploy to EKS  

Yes, I’ve deployed applications to EKS by containerizing the app, pushing images to ECR, deploying via Kubernetes manifests or Helm, and automating the process using Jenkins with monitoring and scaling enabled.

## Suppose one service is responding slowly while other services are healthy. How would you troubleshoot the issue?

I analyze latency metrics, pod health, logs, and downstream dependencies to isolate the bottleneck, mitigate impact through scaling or rollback, and then apply a permanent fix based on root cause.

## Write a docker file for Nginx
```sh
# Use official Nginx image as base
FROM nginx:latest

# Set working directory inside container
WORKDIR /usr/share/nginx/html

# Remove default Nginx index page
RUN rm -rf ./*

# Copy your custom static website files into the container
COPY ./html/ .

# Expose port 80 for Nginx
EXPOSE 80
```
## Start Nginx in foreground (default command from base image)
CMD ["nginx", "-g", "daemon off;"]

docker build -t my-nginx .docker run -d -p 8080:80 my-nginx


=================================
# docker by ram

docker has two types of images:
1: Raw images: OS minor footprint
2: service images: Ready to use

docker system df 

## docker info |grep -i root >> to know where docker install (path) or document root 

## how to run container using service?

docker container run -d --name=test nginx  >> it will create file system , mountpoint , nginx, ip, vnic nginx. 
docker ps 
ps -ef 
kill -9 ppid
docker start test
docker ps >> to show os process 
docker top test >> to know ps of container 
docker exec -it test bash >> to login container terminal
docker rm test >> to remove conatiner > rmi >> To remove images 
docker inspect test 
docker network ls
docker network inspect bridge |grep -i subnet  
docker stats test( container name) to know the using resources by container

## we can run to nginx server because they are using seprate namespaces they run in same port they are not conflict
docker run -d --name=web1 nginx
docker run -d --name=web2 nginx

## there are two types of partitions lvm and standard partions
lvm we can extend but standard partition we cant extend 


## we can trigger any command without login
docker exec -it web1 touch /roo/abc

## how to run container using service images?

## how to run conatiner using Raw images?
docker run -itd --name=test ubntu  >> we use itd coz it will allow /bin/bash to alive the container
to login Raw container >> docker attach test  
# important point 
if we use docker attach test (conatiner name) once we exit container will stopped bcause we are using exisitng shell. to exit container without stopping use ctl +p+q (linux nohup signal)
## if we use docker exec -it test bash the exit will not stop container.

## In industry we cant directly run the images

First we have nginx 
docker  run -it --name=test imgid bash >> not start native services
docker run -d --name=web imageid  
docker exec -it web bash  

# image create method  
1 manually
Requirements (apache2)
step1 : BaseImage (raw image) ubuntu
step2: Run image or deploy container
step3: attach the container and start the installation software
step 4 : logout the conatienr and commit it inspired by vcs technology like git

docker pull ubuntu
docker run -it --name=demo ubuntu
apt update -y 
apt install apache2 -y 
exit 
docker images 
docker commit demo apache:2
## Their is no benefits of keeping conatiner as we have commited changes those changes has been persistent inside the image if we lost container there is no harm image has configured from that image we can launch many container of apache2

## what is diff bw parent & child 

A parent image is the base image defined by FROM, and a child image is built on top of it by adding application-specific layers.

## updates 
1) Minor (bug/security):vertical practice we follow
2) major update we follow horizotal practice

## there are two ways to free up space 1 > dangle image we can delete & we can delete unused images (whihc is not currently running not associate with container.
## dangle images which is unecessay consuming system space which is deprecated version we have to dlete it

## docker image prune  it only terminate dangle images
## docker image prune -a it will terminate dangle + unused images 

##  what practice i will follow in docker file to decrease no of commit id.
RUN paramter is improper so there are lots of instruction "use && " insrtuctions.

## to free up builder space 
docker system df
docker builder prune ( it delete dangle image cached)
docker builder prune -a ( it delete all (dangle+ regular image)cache)  

## what is issues before multi stage docker file
Before multi-stage Dockerfiles, images were large, insecure, and hard to maintain because build and runtime dependencies were bundled together.  

## docker file

copy . . (1st dot says : Source :current directory on your local machine (build context))

2nd dot says: Destination → current working directory inside the image (WORKDIR)

## to get volume informations
Docker volume ls 
## to know mapped disk
docker volume inspect volume name

## To create file system
docker volume create --device=/dev/device_name --driver local image_name

docker volume create --dev=/dev/device_name --driver local image_name

docker volume create --driver local --opt type=xfs --opt device=/dev/nvmeOn2 oracle

docker volume ls

dcoker volume prune delete unused volume where as -a delete all used as well unused volume 

## to send the data docker cp abc con1:/root

## Docker networking


docker network ls 

brctl sow to show bridge n/w

docker network inspect bridge |grep -i subnet  
by default subnet is : 172.17.0.0/16

docker run -itd --name=con1 alpine  
docker run -itd --name=con2 alpine
docker inspect con2  >> take the ip address of cont1
docker exec -it con1 sh  

## what is public/external ip of container .
Containers do not have a public IP; they use the host’s public IP and are accessed through port mapping or load balancers.

## How many network deriver are there?
majorly 5 drivers are therir by defaut is uses bridge n/w
1 Bridge
2 Host
3 Macvlan
4 Ipvlan
5 None

## To create a custom bridge network 
docker network create --subnet 192.168.10.0/24 --driver bridge front  
docker network create --subnet 192.168.11.0/24 --driver bridge backend
docker network ls  

## it is possibel to create multiple bridge nw in one container 
Every conatiern has gateway  

docker ps 
docker exec -it con2 sh  

## To connect DB to application server 
docker network connect backend web  
docker exec -it web bash 
ifconfig  
telent ip adress of app 3306(db port)
docker network disconnect backend web (connect bridge name and conatiner name)  
docker network ls
docker network prune  

## How to expose bridge network conatiner 

to check the port number of imgae
docker inspect image_id  

docker run -d --name=db -e MYSQL_ROOT_PASSWORD=redhat mysql:5.6

docker ps  

or login to container 
docker exec -it db bash
 apt install net-tools -y 
netstat -tnlp  

## Port forwarding  (-p stand for port forwarding)
docker run -d --name=db -p 3336:3306 -e MYSQL_ROOT_PASSWORD=redhat mysql:5.6  

netstat -tnlp

## two types of port forwarding static & dynamic  ( captal P for dynamic port forwarding small p means static port forwarding)
static means we have to 

docker run -itd --name=con1 --network tst alpine 

docker run -itd --name=con2 --network tst alpine 

docker inspect con1 |grep -i ipaddress

 













