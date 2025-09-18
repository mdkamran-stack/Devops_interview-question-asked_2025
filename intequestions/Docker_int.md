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





