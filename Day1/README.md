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

## Linux Kernel features that enable container technology
- Linux Kernel supports the below features which are used by container softwares
  1. Namespace
     - helps in isolating one container from the other container by running them in different namespace
  2. Control Group (CGroup)
     - helps in apply resource quota restrictions on containers
     - example
       - we can restrict how many cpu cores at the max a container can use at any point
       - we can restrict the max amount of RAM a container can utilize at any point of time
       - we can restrict the max storage a container can use 

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

## ⛹️‍♀️ Lab - Copying files from local system to container and vice versa
```
echo "My file" > file1.txt
ls
docker cp file1.txt ubuntu1:/
echo "file inside ubuntu1 container" > file2.txt
exit
docker cp ubuntu1:/file2.txt .
ls
```

Expected output
<pre>
[jegan@tektutor ~]$ echo "My file" > file1.txt
[jegan@tektutor ~]$ ls
Desktop    Downloads  google-chrome-stable_current_x86_64.rpm  Pictures  Templates
Documents  file1.txt  Music  

[jegan@tektutor ~]$ docker cp file1.txt ubuntu1:/
Successfully copied 2.05kB to ubuntu1:/
  
[jegan@tektutor ~]$ docker exec -it ubuntu1 bash
root@ubuntu1:/# ls
bin   dev  file1.txt  lib    lib64   media  opt   root  sbin  sys  usr
boot  etc  home       lib32  libx32  mnt    proc  run   srv   tmp  var
  
root@ubuntu1:/# cat file1.txt 
My file
  
root@ubuntu1:/# echo "file from docker container" > file2.txt
root@ubuntu1:/# ls
bin   dev  file1.txt  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  etc  file2.txt  lib   lib64  media   opt  root  sbin  sys  usr
root@ubuntu1:/# exit
exit
  
[jegan@tektutor ~]$ docker cp ubuntu1:/file2.txt .
Successfully copied 2.05kB to /home/jegan/.
  
[jegan@tektutor ~]$ ls
Desktop    Downloads  file2.txt                                Music     Public     Videos
Documents  file1.txt  google-chrome-stable_current_x86_64.rpm  Pictures  Templates
</pre>


## ⛹️‍♂️ Lab - Deleting a single exited container
```
docker rm ubuntu3
```

## ⛹️‍♂️ Lab - Deleting a running container forcibly
Deleting a running container without stopping it would result in an error.  Hence, either we need to stop it before deleting the container or we need to forcibly delete it as shown below.

```
docker rm -f ubuntu2
```

## ⛹️‍♂️ Lab -  Deleting a running container gracefully
```
docker stop ubuntu1
docker rm ubuntu1
```

## ⛹️‍♂️ Lab -  Deleting multiple containers without calling out their names
```
docker rm -f $(docker ps -aq)
```

Expected output
<pre>
[jegan@tektutor ~]$ docker rm -f $(docker ps -aq)
a0aa59478b56
7fe9a87f8843
67e1e8a43c53  
</pre>

## ⛹️‍♂️ Lab -   Building Custom Docker image with the required tools
```
cd ~/openshift-aug-2023
git pull
cd Day1/CustomMavenImageUsingAlpineBaseImage
docker build -t tektutor/alpine-maven:latest .
```

Expected output
<pre>
[jegan@tektutor Day1]$ mkdir CustomMavenImageUsingAlpineBaseImage
[jegan@tektutor Day1]$ cd CustomMavenImageUsingAlpineBaseImage/
[jegan@tektutor CustomMavenImageUsingAlpineBaseImage]$ ls
[jegan@tektutor CustomMavenImageUsingAlpineBaseImage]$ vim Dockerfile
[jegan@tektutor CustomMavenImageUsingAlpineBaseImage]$ docker build -t tektutor/alpine-maven:latest .
[+] Building 16.3s (6/6) FINISHED                                                                docker:default
 => [internal] load build definition from Dockerfile                                                       0.0s
 => => transferring dockerfile: 199B                                                                       0.0s
 => [internal] load .dockerignore                                                                          0.0s
 => => transferring context: 2B                                                                            0.0s
 => [internal] load metadata for docker.io/library/alpine:latest                                           2.9s
 => [1/2] FROM docker.io/library/alpine:latest@sha256:7144f7bab3d4c2648d7e59409f15ec52a18006a128c733fcff2  0.9s
 => => resolve docker.io/library/alpine:latest@sha256:7144f7bab3d4c2648d7e59409f15ec52a18006a128c733fcff2  0.0s
 => => sha256:7144f7bab3d4c2648d7e59409f15ec52a18006a128c733fcff20d3a4a54ba44a 1.64kB / 1.64kB             0.0s
 => => sha256:c5c5fda71656f28e49ac9c5416b3643eaa6a108a8093151d6d1afc9463be8e33 528B / 528B                 0.0s
 => => sha256:7e01a0d0a1dcd9e539f8e9bbd80106d59efbdf97293b3d38f5d7a34501526cdb 1.47kB / 1.47kB             0.0s
 => => sha256:7264a8db6415046d36d16ba98b79778e18accee6ffa71850405994cffa9be7de 3.40MB / 3.40MB             0.4s
 => => extracting sha256:7264a8db6415046d36d16ba98b79778e18accee6ffa71850405994cffa9be7de                  0.4s
 => [2/2] RUN apk add openjdk11 maven                                                                      8.9s
 => exporting to image                                                                                     3.5s 
 => => exporting layers                                                                                    3.5s 
 => => writing image sha256:65ba5804352b94b8843dc71e58dcf5b7a831f36879f63d4d22d2760b1ce34e82               0.0s 
 => => naming to docker.io/tektutor/alpine-maven:latest    
  0.0s 
[jegan@tektutor CustomMavenImageUsingAlpineBaseImage]$ docker images                                            
REPOSITORY              TAG       IMAGE ID       CREATED          SIZE                                          
tektutor/alpine-maven   latest    65ba5804352b   7 seconds ago    283MB
tektutor/maven          latest    e9721a66d076   5 minutes ago    705MB
tektutor/ubuntu         22.04     f39f6625541c   14 minutes ago   123MB
ubuntu                  22.04     01f29b872827   2 weeks ago      77.8MB
hello-world             latest    9c7a54a9a43c   3 months ago     13.3kB  
</pre>

## Setting up a Load Balancer with nginx

Let us create 3 web server containers named web1, web2 and web3 respectively as shown below.

```
docker run -d --name web1 --hostname web1 nginx:latest
docker run -d --name web2 --hostname web2 nginx:latest
docker run -d --name web3 --hostname web3 nginx:latest
```

Expected output
<pre>
[jegan@tektutor ~]$ docker run -d --name web1 --hostname web1 nginx:latest
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
52d2b7f179e3: Pull complete 
fd9f026c6310: Pull complete 
055fa98b4363: Pull complete 
96576293dd29: Pull complete 
a7c4092be904: Pull complete 
e3b6889c8954: Pull complete 
da761d9a302b: Pull complete 
Digest: sha256:104c7c5c54f2685f0f46f3be607ce60da7085da3eaa5ad22d3d9f01594295e9c
Status: Downloaded newer image for nginx:latest
40555a6c201b5cb1eaaed44b880e4787ccdfb9390e98d22328df9e94d285a6e0
  
[jegan@tektutor ~]$ docker run -d --name web2 --hostname web2 nginx:latest
98609cc6879b6eaa8e37db8b7e7b72a102c9a042796d3d9ba0c8b4a75eea3fdc
  
[jegan@tektutor ~]$ docker run -d --name web3 --hostname web3 nginx:latest
3399fbd2873f4b50e77061fe720bd648b9df10b9ab869307d251f7c9f2901be1
</pre>


Let us now create the load balancer container
<pre>  
[jegan@tektutor ~]$ docker run -d --name lb --hostname lb nginx:latest
61a98cdbdcb2017d54600979acc27861f1998279f25f3877a01bd841165fbbbd
  
[jegan@tektutor ~]$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS     NAMES
61a98cdbdcb2   nginx:latest   "/docker-entrypoint.…"   2 seconds ago    Up 1 second     80/tcp    lb
3399fbd2873f   nginx:latest   "/docker-entrypoint.…"   13 seconds ago   Up 12 seconds   80/tcp    web3
98609cc6879b   nginx:latest   "/docker-entrypoint.…"   18 seconds ago   Up 17 seconds   80/tcp    web2
40555a6c201b   nginx:latest   "/docker-entrypoint.…"   25 seconds ago   Up 24 seconds   80/tcp    web1
</pre>

The lb container must be configured to work like a Load Balancer, as it works by default as a web server.

The original nginx.conf file looks as shown below.  You may optionally copy it from lb container to local machine to check it yourself.
```
docker cp lb:/etc/nginx/nginx.conf .
cat nginx.conf
```

Expected output
```
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

The above file must be modified as shown below
```

user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
	upstream backend {
		server 172.17.0.2:80;
		server 172.17.0.3:80;
		server 172.17.0.4:80;
	}

	server {
		location / {
			proxy_pass http://backend;
		}
	}
}
```
In the above file, IP addresses below to web1, web2 and web3 containers.

You my check the IP address of your web1, web2, web3 and lb containers as shown below and modify the above file accordingly
```
docker inspect web1 | grep IPA
docker inspect -f {{.NetworkSettings.IPAddress}} web2
docker inspect -f {{.NetworkSettings.IPAddress}} web3
docker inspect -f {{.NetworkSettings.IPAddress}} lb
```

Expected output
<pre>
[jegan@tektutor ~]$ docker inspect web1 | grep IPA
            "SecondaryIPAddresses": null,
            "IPAddress": "172.17.0.2",
                    "IPAMConfig": null,
                    "IPAddress": "172.17.0.2",
  
[jegan@tektutor ~]$ docker inspect -f {{.NetworkSettings.IPAddress}} web2
172.17.0.3
  
[jegan@tektutor ~]$ docker inspect -f {{.NetworkSettings.IPAddress}} web3
172.17.0.4
  
[jegan@tektutor ~]$ docker inspect -f {{.NetworkSettings.IPAddress}} lb
172.17.0.5  
</pre>  

<pre>
[jegan@tektutor ~]$ docker inspect web1 | grep IPA
            "SecondaryIPAddresses": null,
            "IPAddress": "172.17.0.2",
                    "IPAMConfig": null,
                    "IPAddress": "172.17.0.2",
  
[jegan@tektutor ~]$ docker inspect -f {{.NetworkSettings.IPAddress}} web2
172.17.0.3
[jegan@tektutor ~]$ docker inspect -f {{.NetworkSettings.IPAddress}} web3
172.17.0.4
[jegan@tektutor ~]$ docker inspect -f {{.NetworkSettings.IPAddress}} lb
172.17.0.5  
</pre>

We need to copy the updated nginx.conf back into the lb container as shown below
```
docker cp nginx.conf lb:/etc/nginx/nginx.conf
```

Restart the lb container to apply config changes
```
docker restart lb
```

Check if the lb container is still running after config changes are applied
```
docker ps
```

Assuming, everything worked as expected so far, let's customize the web1, web2 and web3 index.html pages in order to visualize the LB container performing round-robin load-balancing.

```
echo "Nginx Web server 1" > index.html
docker cp web1:/usr/share/nginx/html/index.html

echo "Nginx Web server 2" > index.html
docker cp web2:/usr/share/nginx/html/index.html

echo "Nginx Web server 3" > index.html
docker cp web3:/usr/share/nginx/html/index.html
```

Now you may try accessing the lb IP address from your centos lab machine web browser
<pre>
http://172.17.0.5:80  
</pre>

If you try the same from your RPS windows lab machine it shouldn't work. In order to make that work, we must do port-forwarding. Let's delete the lb container
```
docker rm -f lb
```

Let's recreate a new lb container, but this time let's setup port forwarding as shown below
```
docker run -d --name lb --hostname lb -p 80:80 nginx:latest
```

The port 80 on the left side is exposed on the local machine, while the port 80 on the right side is where the nginx web server is listening interanally on the container.

We need to copy the pre-configured nginx.conf file into our new lb container
```
docker cp nginx.conf lb:/etc/nginx/nginx.conf
docker restart lb
```
Now you may try accessing the lb IP address from your centos lab machine web browser
<pre>
http://172.17.0.5:80  
</pre>

Now you may also try accessing the lb setup from your RPS windows lab machine using the IP address of your CentOS lab machine
```
http://192.168.1.108:80
```

You will have to find the IP address of your CentOS lab machine and change the above IP accordingly.

The expected output is
<pre>
[jegan@tektutor NginxLBConfiguration]$ curl 172.17.0.5
Nginx Web Server 1
	
[jegan@tektutor NginxLBConfiguration]$ curl 172.17.0.5
Nginx Web Server 2
	
[jegan@tektutor NginxLBConfiguration]$ curl 172.17.0.5
Nginx Web Server 3	
	
</pre>
