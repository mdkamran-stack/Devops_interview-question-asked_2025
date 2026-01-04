## Diff b/w copy vs add ?

Copy is used to copy a file or dicrectory from host machine to docker image.  
ADD instructions i used for extracting tar ball and copying remote url  
Recommedned copy  

## what is persistent volume in docker.

1 volume wheb we use volume option it will create new volume in /var/lib/docker directory and data will be saved in remote machine f we removed container we can attach one volume to multiple conatiner
2 Bind mount a file & dir from a host machine is mounted into a container.

## What is docker file & its instructions.
Docker file is text file which uses intruction to assemble docker images, we can custom acc to our need.  

1 FROM: docker file is starts with from it is first inst whic create base image layer, after that each inst is create further layer.  
2 COPy : is used to copy afile from host machine to docker image.  
3 RUN : it is used to run cmd inside the image.  
4 ENV: it sets the ENV varible that can be used inside the docker file.  
5 CMD : it specify what command start when conatiner starts, just like cmd we can use ENTRY point to do same job.  
 
WORKDIR  sets the working directory inside the container where commands will run.  

ENRTPOINT VS CMD: CMD when you want a default command that users can override. Use ENTRYPOINT when you want a fixed command, and let users only supply arguments.

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

## A container crashed. How do you figure out what went wrong?

Use docker logs <container_id> to view logs. Run docker inspect to get the exit code and details. Check for memory limits, incorrect entrypoints, missing files, or permission issues. In Kubernetes, also use kubectl describe pod and kubectl logs.

## How do you troubleshoot failed Docker image push to registry? 
Answer: Check registry credentials → Validate image name/tag → Ensure repository exists → Retry with correct login. 

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
1 Raw images: OS minor footprint
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

# docker image prune  it only terminate dangle images
# docker image prune -a it will terminate dangle + unused images 






