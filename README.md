# Docker Basis Commands

Docker user containers </br> 
A container has <b>code, configs, environment, libraries/dependencies, runtime </b>

# Docker Architecture Overview

Docker is based on **client-server architecture**, where it uses a client to communicate with the Docker daemon, which manages containers, images, and other components. This README provides a detailed explanation of Docker's architecture and components.

---

## 1. Docker Components

Docker consists of the following core components:

1. **Docker Client (CLI)**  
   - Used to issue commands (`docker run`, `docker build`, etc.)  
   - Communicates with the Docker daemon using a REST API over UNIX socket or network.

2. **Docker Daemon (dockerd)**  
   - Runs on the host machine.  
   - Listens to Docker API requests from the client.  
   - Manages Docker objects like containers, images, volumes, and networks.  
   - Builds and runs containers.  

3. **Docker Objects**  
   - **Images**: Blueprint for containers; contains code, runtime, libraries, etc.  
   - **Containers**: Running instances of Docker images.  
   - **Volumes**: Persistent data storage for containers.  
   - **Networks**: Allow communication between containers and external systems.

4. **Docker Registry**  
   - A storage and distribution system for Docker images.  
   - **Docker Hub** is the default public registry, but private registries can be used.  
   - The client pulls images from or pushes images to the registry.

---

## 2. Docker Architecture Layers

### High-level Structure:
1. **Docker Engine (Docker Daemon)**  
   Consists of three main parts:  
   - **Docker Server**: Accepts client commands and communicates with the system.  
   - **REST API**: Provides programmatic access to Docker features.  
   - **Container Runtime**: Creates and manages containers.

2. **Underlying Components**  
   Docker runs on the host system and utilizes the following:  
   - **Namespaces**: Isolates resources like process IDs, network, and mounts for each container.  
   - **Control Groups (cgroups)**: Limits resources like CPU and memory for each container.  
   - **Union File System (UFS)**: Enables layers in Docker images for efficient storage and image sharing.

---

## 3. Detailed Docker Workflow

Here’s how the Docker architecture works in a typical container lifecycle:

1. **Pulling an Image**  
   The Docker client sends a request to the daemon to pull an image from a registry. The daemon downloads it and caches it locally.

2. **Creating a Container**  
   - The client requests the daemon to create a container from a pulled image.  
   - The container is created with its own isolated filesystem using namespaces and cgroups for resource control.

3. **Running a Container**  
   - The daemon starts the container and manages its lifecycle (start, stop, restart, etc.).  
   - Containers can be connected to networks, mounted with volumes for persistent storage, and exposed on specific ports.

4. **Managing Containers**  
   The Docker client can interact with running containers to execute commands, monitor logs, and manage resources.

---


## 4. Docker Architecture Diagram
![Docker Architecture Diagram](https://media.geeksforgeeks.org/wp-content/uploads/20221205115118/Architecture-of-Docker.png)

## 5. Docker Workflow
![Docker Workflow Diagram](https://file.notion.so/f/f/84ad6f6a-681d-4a55-a9be-d328db326720/97c98574-bd58-4ca7-9944-d2166d4402d1/Untitled.png?table=block&id=5c2a012e-c896-4ede-9d47-850d0c9645df&spaceId=84ad6f6a-681d-4a55-a9be-d328db326720&expirationTimestamp=1738965600000&signature=s0DkAfPdKhkF_WEP1RUuPDc1OJIMrTZvlmPeEubtu_s&downloadName=Untitled.png)


## Containerized a TO-DO NodeJS App

1. **Create a Dockerfile**
   - Used to write instructions for creation <br> `FROM`	Create a new build stage from a base image.
   - Go to dockerhub to find the available public registries like for NODEJS, JAVA etc
   - Once the base image is set now its time to set the working Directory

2. **Dependencies**
   - Copy package.json
   - We need to npm to run the app so add npm in the docker file so step 1 is to run a command npm install to install all dependencies
   - Once all these is ready im gonna copy all these to my container
   - Define the network ports that this container will listen on at runtime.
   - `CMD [ "executable", "parameter", ... ]`
   The default executable for this executing container.

   Set the default executable and parameters for this executing container.


3. **Now we need to run command to create image out of this docker file**
   - `docker buid`-t [nameofimage] [pathofdockerfile]

   - `docker images` command that shows you list of images
   - i can now push this to docker container or docker hub
   - to run image as a docker container `docker run` we can pass params like -it for interactive shell -d to run in background -p to map the ports
   - `docker run -it -d -p 3000:3000 mytodoapp` means run it on my local 3000 and 3000 on docker
   - check using `docker ps` to get list of all running containers

4. **You can use `docker init`to automatically create these template blank files**

5. **Pushing this image on dockerhub(public) and ECRs like(private on AWS)**
- Create a docker hub Account and create a repositiory on it like github then use `docker login` to connect docker with terminal
- Now change the name of the image else docker hub won't recognise it use `docker tag currName username/newname` ->
`docker tag mytodoapp aayush256/mytodoapp256`
- Now to push this `docker push aayush256/mytodoapp256`

## ALL of this process is Automated using CI/CD Pipelines

- To pull this image all you need is `docker pull <'imagename'>` and start running it using `docker run`

- to stop it use `docker stop id` and to delete `docker rm id`

- to view the stopped containers `docker ps -a` command but this won't show the deleted containers

- the command `docker rmi id` to delete the images






