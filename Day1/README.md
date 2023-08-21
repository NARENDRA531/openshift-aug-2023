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

## ⛹️‍♂️ Lab -  Docker Info
```
docker info
```

Expected output
<pre>
[jegan@tektutor ~]$ docker info
Client: Docker Engine - Community
 Version:    24.0.5
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.11.2
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.20.2
    Path:     /usr/libexec/docker/cli-plugins/docker-compose

Server:
 Containers: 5
  Running: 2
  Paused: 0
  Stopped: 3
 Images: 2
 Server Version: 24.0.5
 Storage Driver: overlay2
  Backing Filesystem: xfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 8165feabfdfe38c65b599c4993d227328c231fca
 runc version: v1.1.8-0-g82f18fe
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
 Kernel Version: 3.10.0-1160.el7.x86_64
 Operating System: CentOS Linux 7 (Core)
 OSType: linux
 Architecture: x86_64
 CPUs: 48
 Total Memory: 125.4GiB
 Name: tektutor
 ID: b9c79fd4-1471-4661-ba78-66d75bdd39dd
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
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

## ⛹️‍♂️ Lab - Renaming a container
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


## ⛹️‍♂️ Lab - Deleting a container
```
docker ps -a
docker rm hello_container1
docker ps -a
```

Expected output
<pre>
[jegan@tektutor ~]$ docker ps -a
CONTAINER ID   IMAGE                COMMAND    CREATED             STATUS                         PORTS     NAMES
3f058eceb743   hello-world:latest   "/hello"   About an hour ago   Exited (0) About an hour ago             hello_container1
  
[jegan@tektutor ~]$ docker rm hello_container1 
hello_container1
  
[jegan@tektutor ~]$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES  
</pre>


## ⛹️‍♂️ Lab - Creating docker containers with specific names and hostnames
```
docker ps -a
docker run --name hello1 --hostname hello1 hello-world:latest
docker run --name hello2 --hostname hello2 hello-world:latest
docker ps -a
```

Expected output
<pre>
[jegan@tektutor ~]$ <b>docker ps -a</b>
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[jegan@tektutor ~]$ <b>docker run --name hello1 --hostname hello1 hello-world:latest</b>

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

[jegan@tektutor ~]$ <b>docker run --name hello2 --hostname hello2 hello-world:latest</b>

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

[jegan@tektutor ~]$ <b>docker ps -a</b>
CONTAINER ID   IMAGE                COMMAND    CREATED          STATUS                      PORTS     NAMES
7fe9a87f8843   hello-world:latest   "/hello"   8 seconds ago    Exited (0) 7 seconds ago              hello2
67e1e8a43c53   hello-world:latest   "/hello"   15 seconds ago   Exited (0) 14 seconds ago             hello1  
</pre>

## ⛹️‍♂️ Lab - Creating containers in the background/deattached mode
```
docker run -dit --name ubuntu1 --hostname ubuntu1 ubuntu:22.04 /bin/bash
docker run -dit --name ubuntu2 --hostname ubuntu2 ubuntu:22.04 /bin/bash
docker ps
```
In the above command,
<pre>
d - stands for deattached or background
i - interactive
t - terminal
</pre>

Expected output
<pre>
[jegan@tektutor ~]$ docker run -dit --name ubuntu1 --hostname ubuntu1 ubuntu:22.04 /bin/bash
264695929f6fecbf5453b2b8072e87ce8a29036c648854a96a305dda9354c61f
  
[jegan@tektutor ~]$ docker run -dit --name ubuntu2 --hostname ubuntu2 ubuntu:22.04 /bin/bash
9882f69ac6534523c0ee89ec05452669ab1ffa31f51b4de976e051894ba57992
  
[jegan@tektutor ~]$ docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS          PORTS     NAMES
9882f69ac653   ubuntu:22.04   "/bin/bash"   2 seconds ago    Up 1 second               ubuntu2
264695929f6f   ubuntu:22.04   "/bin/bash"   12 seconds ago   Up 11 seconds             ubuntu1 
</pre>


## ⛹️‍♀️ Lab - Getting inside a container that runs in background
```
docker ps
docker exec -it ubuntu1 /bin/bash
hostname
hostname -i
ls
exit
```

Expected output
<pre>
[jegan@tektutor ~]$ <b>docker ps</b>
CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS          PORTS     NAMES
9882f69ac653   ubuntu:22.04   "/bin/bash"   2 seconds ago    Up 1 second               ubuntu2
264695929f6f   ubuntu:22.04   "/bin/bash"   12 seconds ago   Up 11 seconds             ubuntu1
  
[jegan@tektutor ~]$ <b>docker exec -it ubuntu1 /bin/bash</b>
  
root@ubuntu1:/# <b>hostname</b>
ubuntu1
  
root@ubuntu1:/# <b>hostname -i</b>
172.17.0.2
  
root@ubuntu1:/# <b>ls</b>
bin   dev  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  etc  lib   lib64  media   opt  root  sbin  sys  usr
  
root@ubuntu1:/# <b>exit</b>
exit  
</pre>

## ⛹️‍♀️ Lab - Creating and starting a container in foreground/interactive mode
```
docker run -it --name ubuntu3 --hostname ubuntu3 ubuntu:22.04 /bin/bash
ls
hostname -i
hostname
exit
docker ps -a
```

Expected output
<pre>
[jegan@tektutor ~]$ <b>docker run -it --name ubuntu3 --hostname ubuntu3 ubuntu:22.04 /bin/bash</b>
  
root@ubuntu3:/# <b>ls</b>
bin   dev  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  etc  lib   lib64  media   opt  root  sbin  sys  usr
  
root@ubuntu3:/# <b>hostname -i</b>
172.17.0.4
  
root@ubuntu3:/# <b>hostname</b>
ubuntu3
  
root@ubuntu3:/# <b>exit</b>
exit
  
[jegan@tektutor ~]$ <b>docker ps -a</b>
CONTAINER ID   IMAGE                COMMAND       CREATED          STATUS                      PORTS     NAMES
a0aa59478b56   ubuntu:22.04         "/bin/bash"   49 seconds ago   Exited (0) 6 seconds ago              ubuntu3
9882f69ac653   ubuntu:22.04         "/bin/bash"   11 minutes ago   Up 11 minutes                         ubuntu2
264695929f6f   ubuntu:22.04         "/bin/bash"   11 minutes ago   Up 11 minutes                         ubuntu1
7fe9a87f8843   hello-world:latest   "/hello"      17 minutes ago   Exited (0) 17 minutes ago             hello2
67e1e8a43c53   hello-world:latest   "/hello"      17 minutes ago   Exited (0) 17 minutes ago             hello  
</pre>
