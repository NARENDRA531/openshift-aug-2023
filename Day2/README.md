# Day 2

## What is Container Runtime?
- is the software that manages the containers
  - create container
  - list container
  - delete container
  - modify container
  - stop/stop/restart/kill/abort containers
  - normally end-users don't use container runtimes directly as they are very low-level
  - hence normally container runtimes are used by container engines
- examples
  - runC, CRI-O

## What is a Container Engine?
- a high-level software that supports user-friendly commands to manage images and containers
- Container Engine depends on Container Runtime to manage containers
- Similarly Container Engines depends on other tools to manage Images, etc.,
- Examples
  - Docker 
  - Podman
- Docker Container Engine depends on containerd which internally depends on runC container runtime
- Podman Container Engine depends on CRI-O container runtime
  
## Orchestration Platform Overview
- Benefits of using Orchestration Platform
  - manages your containerized application workloads
  - helps you expose your application workloads as services internally or externally based on your needs
  - provides an environment that helps you make your application highly available (HA)
  - supports in-built monitoring tools to check your application health/live etc.,
  - supports load balancing out of the box, at the same allows you to use external load balancer if there is a need
  - supports Scale up/down
    - scale up - whenever there is too much user traffic to your application, orchestration platform can help you increase the number of application instances
    - scale down - whenever the user traffic to your application comes down, orchestration platform can help you reduce the number of application instance to an appropriate required instances
  - rolling update
    - this feature allow to you upgrade/downgrade your appliction from one version to other version on the live prod environment without any downtime
- Examples of Container Orchestration Platform
  1. Docker SWARM
  2. Google Kubernetes
  3. Red Hat OpenShift
   
## Docker SWARM Overview
- Docker Inc's native Orchestration Platform
- it only supports managing Docker containerized application workloads
- it is not production grade
- good for learning, protyping, R&D, Dev/QA environments
- light-weight compared to Kubernetes/OpenShift

## Kubernetes Overview
- It is an opensource Container Orchestration Platform from Google
- it is a production grade Orchestration Platform
- it support many container runtimes/engines including Docker, containerd, lxc, etc.
- Kubernetes support any container runtimes that implements CRI (Container Runtime Interface)
- Kubernetes cluster - a group of servers works together
  - some of those servers could act like a Master node
  - some of those servers could act like a Worker node
  - some of those servers might support both Master and work role
  - Master/Worker Nodes could be Physical Servers, Virtual Machines,AWS ec2 instances, Azure Virtual Machines, etc.,
  - could have any number of master nodes and worker nodes - generally 3 masters and 2 or more workers are recommended
- Kubernetes aka K8s
- K8s tools - kubectl, kubeadm, kubelet
  - kubectl - is the client tool which end-users will be using to deploy/manage their application workloads
  - kubeadm
    - is an administration tool to manage K8s cluster
    - is used to bootstrap/setup master nodes 
    - is used to add new nodes to the K8s cluster
    - is used to remove existing nodes from K8s cluster
    - is used to join a worker node to the K8s cluster or master nodes
  - kubelet
    - is a Container Agent that runs in every nodes ( master and worker nodes )
    
- Master Node Components
  - in the master nodes, a special set of components run they are called as Control Plane
  - kubelet
  - What are the Control Plane components
    - API Server
    - etcd - key/pair datastore
    - Scheduler
    - Controller Managers
      - Deployment Controller
      - ReplicaSet Controller
      - Node Controller
      - DaemonSet Controller
      - Job Controller
      - CronJob Controller
      - StatefulSet Controller
      - Endpoint Controller
  - Container Runtime
  - CoreDNS
    - supports Service Discovery
      - helps in locating a service by its name as opposed to IP Address
    
- Worker Node Components
  -  Container Runtime ( runC or CRI-O, etc., )
  -  kubelet - Container Agent
  -  CoreDNS
  -  User Applications

- Kubernetes Custom Resource Definition (CRD)
   - through this we can add our own Custom Resources within K8s
   - using this we can extend existing K8s with many additional features

- Kubernetes Custom Controllers
  - in order to manage our custom resources, we also need to supply/implement Custom Controllers

- Kubernetes Operators
  - this is a combination of many Custom Resources with Custom Controllers
  - many applications can be deployed as Kubernetes Operators

## Kubernetes Resources
- Kubernetes support many inbuilt Resources
  - Pod ( group of containers )
    - a JSON configuration object that is stored within etcd datastore
    - applications run within the Pod, to be more-precise, applications run inside some container that is part of the 
      pod
    - recommended best practices insist that only one main application exists per Pod
    - IP address is assigned on the Pod level, hence all containers in the same Pod shares the same IP address
    - Kubernetes uses pause containers to provide network support for all the other containers in the Pod
 - ReplicaSet
   - is a JSON configuration object that is persisted within etcd datastore
   - ReplicaSet is the one which tells how many instances of a Pod is supposed to running at any point of time on a particular application Deployment
   - Each ReplicaSet represents a group of Pods
   - Each ReplicaSet manages a single version of Application Pods
 - Deployment
   - is a JSON configuration object that is persisted within etcd datastore
   - each Deployment has one or more ReplicaSet
   - Whenever we deployment an application into K8s cluster, a Deployment object/resource is created, as part of Deployment a ReplicaSet is created, as part of ReplicaSet Pod(s) are created

   
## Red Hat OpenShift Overview
- Red Hat OpenShift is developed on top of opensource Google Kubernetes
- this is an enterprise product, we need buy license to use it for commercial purpose
- supported by Red Hat ( an IBM company )
- this provides Command Line Interface just like Kubernetes and also supports Webconsole unlike K8s
- supports all features of K8s + many additional features 
