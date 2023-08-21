# Day 1

## Installing Docker in CentOS 7.x
```
su -
yum install -y yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
systemctl enable docker
systemctl start docker
systemctl status docker
usermod -aG docker $USER
exit
```

## Processors with Multiple CPU Cores
<pre>
- Intel/AMD Processors supports multiple cores on a single Processor
- Server grade motherboards support multiple Processor sockets
- Motherboards with 8 Processor Sockets
- Processors comes in 2 form factors
  - SCM(Single Chip Module - 1 IC will contain just one Processor with many cores)
  - MCM(Multiple Chip Module - 1 IC will contain many Processors  with many cores)
- Assume you have a server with 8 Processor Sockets
- In that if you install AMD MCM IC with 4 Processors /per IC
-  Assuming each Processor supports 128 Physical cores
- Each Physical core will be seen as 2 to 4 virtual cores - Hyperthreading

- 8 SCM Processors installed on 8 Sockets, each processor supports 128 CPU Cores
- 128 * 2 * 8 = 2048 Virtual Cores
</pre>

## What is Hypervisor?
- Hypervisor is nothing but Virtualization Technology
- Virtualization technolgy is hardware + software technology
- this type of virtualization is considered as heavy weight virtualization as each VM must be allocated with dedicated CPU cores, RAM and storage
- General Purpose Processors
  - AMD
    - Virtualization feature - AMD-V
  - Intel
    - Virtualization feature - VT-X
- There are two types
  - Type 1 ( Bare Metal Hypervisor  - Servers and Workstation - doesn't require OS to install the hypervisor )
    Examples
    - VMWare vSphere/vCenter
  - Type 2 ( Desktop, Laptop, Workstation - this requires a Host OS to install hypervisor on top of it )
    Examples
    - VMWare - Fusion, Workstation
    - Oracle - VirtualBox
    - Parallels
    - KVM
    - Hyper-V
  - each Virtual Machine(VM) represents one Operating System with dedicated Hardware resources
      
## What is Docker?
- is an application virtualization technology
- this is light weight virtualization technology as they don't need dedicated hardware resources
- each container represents on running application
- Each container get its own IP address just like Virtual Machines
- Each container has its own file system just like Virtual Machines
- Each container has its own Network Stack just like Virtual Machines
- Containers are not Operating System, they don't have OS Kernel
- 
## High Level Architecture of Hypervisor

## High Level Architecture of Docker

## What is a Docker Image?
- a specification of Docker Container
- Whatever tools are required in the containers are installed on the Docker image
- Docker containers are created using Docker images
- Docker images are similar to VMWare images or Windows 10/11 ISO images
- Just like we are able to install any number of Windows 10 OS with Windows 10 ISO image, we can create any number of mysql containers with mysql docker image

## What is a Docker Container?
- is the running instance of a Docker Image
- each Container get its own IP address, Network Stack, File System
- each container represents one application
- though running many applications per container is possible, it is considered a bad practice as it uses supervisord utility to spin off child process and monitors them constantly, which is overhead and defeats the purpose of containers
- 
# Docker Commands

### ⛹️‍♂️ Lab - Checking the Docker version installed on your system
```
docker --version
```
Expected output
<pre>
[jegan@tektutor ~]$ <b>docker --version</b>
Docker version 24.0.5, build ced0996
</pre>

## ⛹️‍♂️ Lab - Creating your first Docker container
```
docker run hello-world:latest
```

Expected output
<pre>
[jegan@tektutor ~]$ <b>docker run hello-world:latest</b>
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete 
Digest: sha256:dcba6daec718f547568c562956fa47e1b03673dd010fe6ee58ca806767031d1c
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
</pre>

## ⛹️‍♂️ Lab - Listing containers

Listing only the running container
```
docker ps
```

Expected output
<pre>
[jegan@tektutor ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES  
</pre>

Listing all containers
```
docker ps -a
```

Expected output
<pre>
[jegan@tektutor ~]$ docker ps -a
CONTAINER ID   IMAGE                COMMAND    CREATED         STATUS                     PORTS     NAMES
3f058eceb743   hello-world:latest   "/hello"   6 minutes ago   Exited (0) 6 minutes ago             frosty_tesla
</pre>

## Lab - Renaming a container
```
docker ps -a
docker rename <current-container-name> <new-container-name>
docker ps -a
```

Expected output
<pre>
[jegan@tektutor ~]$ docker ps -a
CONTAINER ID   IMAGE                COMMAND    CREATED             STATUS                         PORTS     NAMES
3f058eceb743   hello-world:latest   "/hello"   About an hour ago   Exited (0) About an hour ago             frosty_tesla
  
[jegan@tektutor ~]$ docker rename frosty_tesla hello_container1

[jegan@tektutor ~]$ docker ps -a
CONTAINER ID   IMAGE                COMMAND    CREATED             STATUS                         PORTS     NAMES
3f058eceb743   hello-world:latest   "/hello"   About an hour ago   Exited (0) About an hour ago             hello_container1  
</pre>
