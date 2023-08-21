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

### Checking the Docker version installed on your system
```
docker --version
```

Expected output
<pre>
[jegan@tektutor ~]$ docker --version
Docker version 24.0.5, build ced0996
</pre>

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
## What is Docker?

## High Level Architecture of Hypervisor

## High Level Architecture of Docker


