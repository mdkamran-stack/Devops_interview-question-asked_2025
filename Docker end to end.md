## What is Docker?
Docker is an open-source platform where developers can containerize the application  

## What is Dockerfile?  

The Dockerfile uses DSL (Domain Specific Language) and contains instructions for generating a Docker image.  

## What is Docker Image? 

👉 “A Docker image is a read-only template that contains the application code and all its dependencies. It is used to create Docker containers, which are the running instances of the image.”

## What is Docker Container? 
A container is a runtime instance of an image.


## Dockerfile commands/Instructions

### Option for persistent storage in docker:

there is 2 option 1 . Volume  & 2 Bind mount 

## Volume → Docker-managed storage, portable, good for production.

## Bind Mount → Host directory mapped inside container, good for dev, less portable.

## 4 Mind sharing of docker file :

Docker file is text file that uses to assemble docker image , docker image is cutom image that we can build acc to our needs .

There are couple of instructions 

## Docker file is text file is uses instruction to build docker image, it is custom image we can build accourding to our need.

## 1: FROM >> it is first insrction which cretae base image layer after that each instruction add further layers.

## 2: COPY : which copy a file from a host m/c to Image.

## 3: RUN : it is used to run a cmd inside an image

## 4: ENV it set the evironemtal variable that can be used inside docker file.

## 5: CMD it specify what command runs when the container starts Just the the "ENTRYPOINT" to do the same job.

## Diff b/w add and copy >> both add and copy instruction servers for same purpose copy a file but as per official recommendation to use copy instaed of add coz its more transparent and starightforward the copy inst is used to copy a file or directoy from our host m/c into docker image.
## ADD instruction has additonal capabilities such as extracting tar ball & cpying remote URL:

