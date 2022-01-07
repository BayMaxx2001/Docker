# What is Docker! 

#### **Docker** is a software development platform for virtualization with multiple Operating systems running on the same host. Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings. Container images become containers at runtime and in the case of Docker containers - images become containers when they run on [Docker Engine](https://www.docker.com/products/container-runtime)
## Docker vs Virtual Machine 
Docker is container based technology and containers are just user space of the operating system. In Docker, the containers running share the host OS kernel.A container is just a set of processes that are isolated from the rest of the system, running from a distinct image that provides all files necessary to support the processes
Virtual Machine are made up of user space plus kernel space of an operating system. Under VMs, server hardware is virtualized. The hypervisor allows multiple VMs to run on a single machine. Each VM includes a full copy of an operating system, the application, necessary binaries and libraries - taking up tens of GBs **Conclusion** 
1. Docker Containers occupy less space than VMs
2. Containers have a better performance than VMs
3. Docker easy to scale up 
4. High efficiency 
5. Easily portable across different platforms
6. Data volumes can be shared and reused among multiple containers
## Why we need Docker?
1. Docker helps to package and ship , run apllication more easily in everywhere. 
2. Microservices : 
	* Immutable: behavior in local same   other environment 
	* Lightweight: Fast create containers 
	* Stateless: disposable and ephemeral 
## Docker : The components parts

**Docker Engine**  is the core of Docker. It is the underlying client-server technology that creates and runs the containers. The Docker Engine includes a long-running daemon process called dockerd for managing containers, APIs that allow programs to communicate with the Docker daemon, and a command-line interface.Users use Docker-client (CLI) to communicate to Docker deamon by HTTPS protocol

**Dockerfile.** Each Docker container starts with a Dockerfile. This text file provides a set of instructions to build a Docker image step by step, including the operating system, languages, environmental variables, file locations, network ports, and any other components it needs to run.

**Docker Compose.** Docker Compose is a command-line tool that uses YAML files to define and run multicontainer Docker applications. It allows you to create, start, stop, and rebuild all the services from your configuration and view the status and log output of all running services

**Docker image.** Similar to a snapshot in the VM world, a Docker image is a portable, read-only, executable file containing the instructions for creating a container and the specifications for which software components the container will run and how

**Docker Containers.** are the organizational units and one of the Docker basics concept. When we build an image and start running it; we are running in a container. The container analogy is used because of the portability of the software we have running in our container

**Docker registry** Docker stores the images we build in registries. There are public and private registries. Docker company has public registry called [Docker hub](https://hub.docker.com/), where you can also store images privately
**Docker Volume.** Volume indicates the partition of memory that Docker uses to persist data inside container
# Get Started with Docker
### **I. Download and install Docker at: [docker](https://docs.docker.com/engine/install/)**
### **II.  How to use Docker using basic Docker CLI**
```command
$ sudo docker <componets> <command>
```
< componets >
* images
* container
* network
*  ...

< command >
* ls 
* run
* exec
* stop
* pull
* ...

**Example**: 
```command
$ sudo docker info
```
As we can see in the above Docker example, we have information about docker containers how many are running, paused or stopped and how many images we have downloaded. So let’s get our first image in this Docker commands tutorial.

```
$ sudo docker pull alpine
```
With this command we are telling docker to download the image alpine, to pull it from the public registry, the latest version which is set by default.
*alpine is a minimal Docker image based on Alpine Linux with a complete package index and only 5 MB in size.
If we want to run the image as a container, we will use the following command in this Docker tutorials guide.

```
$ sudo docker run -i -t alpine /bin/bash
```
If we run the command, we will be sent directly to the alpine’s terminal. The -i flag keeps STDIN open from the container, even when you are not attached to it. This persistent standard input is one half of what you require for an interactive shell. The -t flag is the other half and which instructs Docker to assign a pseudo-tty to the container. This offers us an interactive shell in the new container. We exit the container with a simple exit command.

We can try running an Ubuntu image.
```
$ sudo docker run -it ubuntu /bin/bash
```
You can notice docker checks for the image locally, and if it’s not there, the image is pulled from the image library automatically, and once again we have an interactive shell running. We can also name the containers as we run them.
```
$ sudo docker run –-name our_container -it ubuntu /bin/bash
```
and we exit again.
We can also run container we previously created, without an interactive shell
```
$ sudo docker start container_name
```
And stop the container writing docker stop container_name
```
$ sudo docker stop container_name
```
If we want to see all running containers, we just run
```
$ docker ps
```
And for all containers we add “- a”at the end of this same command, like this docker ps -a.

This command shows Container’s ID, which image is using when was created, running status, exposed ports and randomly generated name for the container for easier management.
```
$ docker ps -a
```
You can also see which images we have downloaded locally and info about them
```
$ sudo docker images
```
The command in the above Docker example displays the docker image with a tag which shows our image version, a distinctive image ID, when was created and image size

We can use attach - detach mode : detach container form local terminal :
```
$ sudo docker attach <container_id> 
```
to re-attach container stdin and stdout into clocal terminal
 
 Run a container in detach mode 
 ```
 $ sudo docker run -d <image>
 ```
 **Exucute commands insides container**
 Syntax: 
 ```
 $ sudo docker exec <container_id> <command>
```
**Port mapping**
Syntax 
```
$ docker run -p <target_port>:<container_port> ...
```
Example:
```
$ docker run -p 80:80 <docker_image> 
```
**Log trace**
```
$ docker logs -f <container_id>
```
** Volume - bind mount**
```
docker volume create [volume_name]
docker run -v [local_dir/volume]:[container_dir] ...
```
Example
```
docker volume create pgdata
docker run -v pgdata:/var/lib/postgresql/data -p 5432:5432 postgres
```

### **III.  How to build Docker image**
***Build process docker image from Dockerfile***
With each command, when sending build context to Docker Daemon , Docker will create a temporary container from previous layer, then execute command step by step inside that container, take a snapshot of it into a new layer and finally remove the temporary container which is no longer needed
**1. Dockerfile**
Format: 
```dockerfile
INSTRUCTION arguments
FROM ...

RUN <command>(the command is run in a shell, which by default is /bin/sh -c  on Linux or cmd /S /C on Windows)
RUN ["executable", "param1", "param2"]

CMD ["executable","param1","param2"]
CMD ["param1","param2"]
CMD command param1 param

EXPOSE <port> [<port>/<protocol>...]

ENV <key>=<value> ...

ADD [--chown=<user>:<group>] <src>... <dest>
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]

COPY [--chown=<user>:<group>] <src>... <dest>
COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]

WORKDIR /path/to/workdir

```
**a. FROM.** The `FROM` instruction initializes a new build stage and sets the [_Base Image_](https://docs.docker.com/glossary/#base-image) for subsequent instructions. As such, a valid `Dockerfile` must start with a `FROM` instruction

**b. RUN.** The `RUN` instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the `Dockerfile`.

**c. CMD.** There can only be one  `CMD`  instruction in a  `Dockerfile`. If you list more than one  `CMD`  then only the last  `CMD`  will take effect.***The main purpose of a  `CMD`  is to provide defaults for an executing container.*** These defaults can include an executable, or they can omit the executable, in which case you must specify an  `ENTRYPOINT`  instruction as well.

**d. EXPOSE.** The `EXPOSE` instruction informs Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP, and the default is TCP if the protocol is not specified

**e. ENV.** The `ENV` instruction sets the environment variable `<key>` to the value `<value>`.

**f. COPY.** The `COPY` instruction copies new files or directories from `<src>` and adds them to the filesystem of the container at the path `<dest>`.

**g. WORKDIR.** The `WORKDIR` instruction sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instructions that follow it in the `Dockerfile`. If the `WORKDIR` doesn’t exist, it will be created even if it’s not used in any subsequent `Dockerfile` instruction.
Example: 
```dockerfile
FROM golang:alpine
WORKDIR /app
COPY . . 
RUN go mod tidy
RUN  go  build  -o  main  coreservices/cmd/main.go
CMD  ["./main","serve"]
```


**Create image**
Syntax:
```
docker build -t <images_name>:tag .
```
**Build context  -  .dockerignore**

"Build context" refers to the folder that contains Dockerfile. All contents insides that folder is call build context

".dockerignore": It will ignore files and forders listed in the .dockerignore when sending build context to Docker Daemon - same .gitignore

### **IV. Docker Compose**
  
***1. Docker-compose.yml***

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration

  

Compose works in all environments: production, staging, development, testing, as well as CI workflows

  

Using Compose is basically a three-step process:

1. Define your app’s environment with a `Dockerfile` so it can be reproduced anywhere.

  

2. Define the services that make up your app in `docker-compose.yml` so they can be run together in an isolated environment.

  

3. Run `docker compose up` and the [Docker compose command](https://docs.docker.com/compose/cli-command/) starts and runs your entire app. You can alternatively run `docker-compose up` using the docker-compose binary.

  
  

**Command options overview and help**

  

You can also see this information by running

```

docker-compose --help

```

from the command line.

The Compose file is a [YAML](http://yaml.org/) file defining [version](https://github.com/compose-spec/compose-spec/blob/master/spec.md#version-top-level-element) (DEPRECATED), [services](https://github.com/compose-spec/compose-spec/blob/master/spec.md#services-top-level-element) (REQUIRED), [networks](https://github.com/compose-spec/compose-spec/blob/master/spec.md#networks-top-level-element), [volumes](https://github.com/compose-spec/compose-spec/blob/master/spec.md#volumes-top-level-element), [configs](https://github.com/compose-spec/compose-spec/blob/master/spec.md#configs-top-level-element) and [secrets](https://github.com/compose-spec/compose-spec/blob/master/spec.md#secrets-top-level-element)

A `docker-compose.yml` looks like this:
```yaml
version: "3.4"

services:
  app:
    build:
      context: .
    volumes:
      - ./public/images:/app/public/images
      # - ./:/app # mount từ môi trường gốc vào trong để nếu các bạn thay đổi code thì bên trong sẽ tự động cập nhật
    environment:
      - DB_HOST=${DB_HOST}
      - DB_NAME=${DB_NAME}
      - REDIS_HOST=${REDIS_HOST}
      - REDIS_PORT=${REDIS_PORT}
      - PORT=${PORT}
    ports:
      - "${PORT}:${PORT}"
    restart: unless-stopped
    depends_on:
      - redis
      - db
  db:
    image: mongo
    volumes:
      - .docker/data/db:/data/db
    restart: unless-stopped

  redis:
    image: redis:5-alpine
    volumes:
      - .docker/data/redis:/data
    restart: unless-stopped
```
**Service** `Service` is an abstract concept implemented on platforms by running the same container image (and configuration) one or more times.

**build** The `build` element define configuration options that are applied by Compose implementations to build Docker image from source. `build` can be specified either as a string containing a path to the build context or a detailed structure

**config** `configs` grant access to configs on a per-service basis using the per-service `configs` configuration

**depend_on** `depends_on` expresses startup and shutdown dependencies between services

**env_file** `env_file` adds environment variables to the container based on file content

**expose** `expose` defines the ports that Compose implementations MUST expose from container. These ports MUST be accessible to linked services and SHOULD NOT be published to the host machine. Only the internal container ports can be specified

**ports** Exposes container ports. Port mapping MUST NOT be used with `network_mode: host` and doing so MUST result in a runtime error

**restart** `restart` defines the policy that the platform will apply on container termination

**volumes** `volumes` defines mount host paths or named volumes that MUST be accessible by service containers

**name** `name`  set a custom name for this volume. The name field can be used to reference volumes that contain special characters. The name is used as is and will  **not**  be scoped with the stack name


***2. Build image with docker-compose***

Syntax
```
docker-compose build <service_name>
```
Example
```
docker-compose build app
```
***3. Run container with docker-compose***

Syntax
```
docker-compose up
docker-compose up <service_name>
docker-compose up -d <service_name>
docker-compose logs -f <service_name>
docker-compose stop <service_name>
docker-compose down <service_name>
```

# **Commonly used command in Docker**
1. Docker containers
```
    docker ps
    docker ps -a
    docker run -it {image_name}
    docker start/stop -i {container_name}
    docker exec -it {container_id} {command}

    docker exec -it {container_name} /bin/bash 
    docker exec -it {container_name} bash
    docker container rm -f {container_id} //remove container
    docker stop $(docker ps -aq) //stop all container
    docker kill $(docker ps -q) // stop all container
    docker rm $(docker ps -a -q) // remove all container
    docker container prune // remove container not run 

```
2. Docker images: 
```commands
    docker images //list images
    docker pull {image_name} //download image
    docker image ls --all | docker images -a //list all images
    docker rm {image_name} //remove image 
    docker rmi $(docker images -q) //remove all
    
    docker build -t app:v0.0.1 . 
```

3. Docker volume
```
    docker volume ls
    docker volume create {volume_name}
    docker run -it --mount source={disk_name},target={/home/disk} {ubuntu:lastest}
```


4. Combined
```
    docker run --port {port_host}:{port_container} {image_name}
    docker run --name {container_name} {image_name} //rename
    docker run --volume {path_host}:{path_container} {image_name} //mount folder in container with localhost
```

5. Docker-compose
```
    docker-compose build app
    docker-compose up
```
