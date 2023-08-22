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
- in other words is a Red Hat's distribution of Kubernetes
- supports all features of K8s + many additional features 
- oc is the OpenShift client tool equivalent to Kubernetes kubectl
- OpenShift also supports using kubectl client tool

# Red Hat OpenShift commands

## Lab - Listing the OpenShift nodes in the CLI
```
oc get nodes
kubectl get nodes
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get nodes
NAME                         STATUS   ROLES                         AGE     VERSION
master-1.ocp.tektutor.labs   Ready    control-plane,master,worker   5h40m   v1.26.6+6bf3f75
master-2.ocp.tektutor.labs   Ready    control-plane,master,worker   5h39m   v1.26.6+6bf3f75
master-3.ocp.tektutor.labs   Ready    control-plane,master,worker   5h40m   v1.26.6+6bf3f75
worker-1.ocp.tektutor.labs   Ready    worker                        5h23m   v1.26.6+6bf3f75
worker-2.ocp.tektutor.labs   Ready    worker                        5h23m   v1.26.6+6bf3f75
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ kubectl get nodes
NAME                         STATUS   ROLES                         AGE     VERSION
master-1.ocp.tektutor.labs   Ready    control-plane,master,worker   5h40m   v1.26.6+6bf3f75
master-2.ocp.tektutor.labs   Ready    control-plane,master,worker   5h40m   v1.26.6+6bf3f75
master-3.ocp.tektutor.labs   Ready    control-plane,master,worker   5h40m   v1.26.6+6bf3f75
worker-1.ocp.tektutor.labs   Ready    worker                        5h23m   v1.26.6+6bf3f75
worker-2.ocp.tektutor.labs   Ready    worker                        5h23m   v1.26.6+6bf3f75  
</pre>

## Lab - Listing OpenShift nodes with more detailed information
```
oc get nodes -o wide
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get nodes -o wide
NAME                         STATUS   ROLES                         AGE     VERSION           INTERNAL-IP       EXTERNAL-IP   OS-IMAGE                                                       KERNEL-VERSION                 CONTAINER-RUNTIME
master-1.ocp.tektutor.labs   Ready    control-plane,master,worker   5h50m   v1.26.6+6bf3f75   192.168.122.78    <none>        Red Hat Enterprise Linux CoreOS 413.92.202308091852-0 (Plow)   5.14.0-284.25.1.el9_2.x86_64   cri-o://1.26.4-3.rhaos4.13.git615a02c.el9
master-2.ocp.tektutor.labs   Ready    control-plane,master,worker   5h49m   v1.26.6+6bf3f75   192.168.122.3     <none>        Red Hat Enterprise Linux CoreOS 413.92.202308091852-0 (Plow)   5.14.0-284.25.1.el9_2.x86_64   cri-o://1.26.4-3.rhaos4.13.git615a02c.el9
master-3.ocp.tektutor.labs   Ready    control-plane,master,worker   5h50m   v1.26.6+6bf3f75   192.168.122.185   <none>        Red Hat Enterprise Linux CoreOS 413.92.202308091852-0 (Plow)   5.14.0-284.25.1.el9_2.x86_64   cri-o://1.26.4-3.rhaos4.13.git615a02c.el9
worker-1.ocp.tektutor.labs   Ready    worker                        5h33m   v1.26.6+6bf3f75   192.168.122.36    <none>        Red Hat Enterprise Linux CoreOS 413.92.202308091852-0 (Plow)   5.14.0-284.25.1.el9_2.x86_64   cri-o://1.26.4-3.rhaos4.13.git615a02c.el9
worker-2.ocp.tektutor.labs   Ready    worker                        5h33m   v1.26.6+6bf3f75   192.168.122.19    <none>        Red Hat Enterprise Linux CoreOS 413.92.202308091852-0 (Plow)   5.14.0-284.25.1.el9_2.x86_64   cri-o://1.26.4-3.rhaos4.13.git615a02c.el9  
</pre>

## Finding more details about the OpenShift node
```
oc describe node/master-1.ocp.tektutor.labs
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc describe node/master-1.ocp.tektutor.labs
Name:               master-1.ocp.tektutor.labs
Roles:              control-plane,master,worker
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=master-1.ocp.tektutor.labs
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/control-plane=
                    node-role.kubernetes.io/master=
                    node-role.kubernetes.io/worker=
                    node.openshift.io/os_id=rhcos
Annotations:        machineconfiguration.openshift.io/controlPlaneTopology: HighlyAvailable
                    machineconfiguration.openshift.io/currentConfig: rendered-master-eacbf8de7aa07afc156cc5d44dfe6fe5
                    machineconfiguration.openshift.io/desiredConfig: rendered-master-eacbf8de7aa07afc156cc5d44dfe6fe5
                    machineconfiguration.openshift.io/desiredDrain: uncordon-rendered-master-eacbf8de7aa07afc156cc5d44dfe6fe5
                    machineconfiguration.openshift.io/lastAppliedDrain: uncordon-rendered-master-eacbf8de7aa07afc156cc5d44dfe6fe5
                    machineconfiguration.openshift.io/lastSyncedControllerConfigResourceVersion: 19351
                    machineconfiguration.openshift.io/reason: 
                    machineconfiguration.openshift.io/state: Done
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Tue, 22 Aug 2023 08:42:20 +0530
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  master-1.ocp.tektutor.labs
  AcquireTime:     <unset>
  RenewTime:       Tue, 22 Aug 2023 14:52:41 +0530
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Tue, 22 Aug 2023 14:51:56 +0530   Tue, 22 Aug 2023 08:42:20 +0530   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Tue, 22 Aug 2023 14:51:56 +0530   Tue, 22 Aug 2023 08:42:20 +0530   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Tue, 22 Aug 2023 14:51:56 +0530   Tue, 22 Aug 2023 08:42:20 +0530   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Tue, 22 Aug 2023 14:51:56 +0530   Tue, 22 Aug 2023 08:47:17 +0530   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.122.78
  Hostname:    master-1.ocp.tektutor.labs
Capacity:
  cpu:                8
  ephemeral-storage:  104266732Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             15991688Ki
  pods:               250
Allocatable:
  cpu:                7500m
  ephemeral-storage:  95018478229
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             14840712Ki
  pods:               250
System Info:
  Machine ID:                                  426148b58c4f4f57a576a2f517ca4c66
  System UUID:                                 426148b5-8c4f-4f57-a576-a2f517ca4c66
  Boot ID:                                     4e18c041-6f5f-4fdc-a280-8912b24c9c32
  Kernel Version:                              5.14.0-284.25.1.el9_2.x86_64
  OS Image:                                    Red Hat Enterprise Linux CoreOS 413.92.202308091852-0 (Plow)
  Operating System:                            linux
  Architecture:                                amd64
  Container Runtime Version:                   cri-o://1.26.4-3.rhaos4.13.git615a02c.el9
  Kubelet Version:                             v1.26.6+6bf3f75
  Kube-Proxy Version:                          v1.26.6+6bf3f75
Non-terminated Pods:                           (43 in total)
  Namespace                                    Name                                                          CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                                    ----                                                          ------------  ----------  ---------------  -------------  ---
  openshift-apiserver                          apiserver-59cbcb5597-x8gsf                                    110m (1%)     0 (0%)      250Mi (1%)       0 (0%)         5h54m
  openshift-authentication                     oauth-openshift-7bc8477595-rl9mn                              10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         5h57m
  openshift-cloud-controller-manager-operator  cluster-cloud-controller-manager-operator-7d85bfc559-nkhjn    20m (0%)      0 (0%)      75Mi (0%)        0 (0%)         6h9m
  openshift-cluster-node-tuning-operator       tuned-jwbd2                                                   10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         6h2m
  openshift-cluster-storage-operator           csi-snapshot-controller-9584854bf-msfcb                       10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         6h5m
  openshift-cluster-storage-operator           csi-snapshot-webhook-68694997c6-vff8l                         10m (0%)      0 (0%)      20Mi (0%)        0 (0%)         6h5m
  openshift-console-operator                   console-operator-75b68f4466-t984q                             20m (0%)      0 (0%)      200Mi (1%)       0 (0%)         5h58m
  openshift-console                            console-d955f495-qj4ht                                        10m (0%)      0 (0%)      100Mi (0%)       0 (0%)         5h56m
  openshift-console                            downloads-78b784cc4d-k49nx                                    10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         5h58m
  openshift-controller-manager                 controller-manager-6c67dd645d-qmph7                           100m (1%)     0 (0%)      100Mi (0%)       0 (0%)         5h55m
  openshift-dns                                dns-default-g7xnk                                             60m (0%)      0 (0%)      110Mi (0%)       0 (0%)         6h3m
  openshift-dns                                node-resolver-467bm                                           5m (0%)       0 (0%)      21Mi (0%)        0 (0%)         6h3m
  openshift-etcd                               etcd-guard-master-1.ocp.tektutor.labs                         10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         6h2m
  openshift-etcd                               etcd-master-1.ocp.tektutor.labs                               360m (4%)     0 (0%)      910Mi (6%)       0 (0%)         5h51m
  openshift-image-registry                     node-ca-87mfx                                                 10m (0%)      0 (0%)      10Mi (0%)        0 (0%)         5h56m
  openshift-ingress-canary                     ingress-canary-jbpm6                                          10m (0%)      0 (0%)      20Mi (0%)        0 (0%)         5h58m
  openshift-ingress                            router-default-6b9d4d5bfd-mcw2j                               100m (1%)     0 (0%)      256Mi (1%)       0 (0%)         5h56m
  openshift-kube-apiserver                     kube-apiserver-guard-master-1.ocp.tektutor.labs               10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         5h52m
  openshift-kube-apiserver                     kube-apiserver-master-1.ocp.tektutor.labs                     290m (3%)     0 (0%)      1224Mi (8%)      0 (0%)         5h52m
  openshift-kube-controller-manager            kube-controller-manager-guard-master-1.ocp.tektutor.labs      10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         6h2m
  openshift-kube-controller-manager            kube-controller-manager-master-1.ocp.tektutor.labs            80m (1%)      0 (0%)      500Mi (3%)       0 (0%)         5h51m
  openshift-kube-scheduler                     openshift-kube-scheduler-guard-master-1.ocp.tektutor.labs     10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         6h
  openshift-kube-scheduler                     openshift-kube-scheduler-master-1.ocp.tektutor.labs           25m (0%)      0 (0%)      150Mi (1%)       0 (0%)         5h58m
  openshift-kube-storage-version-migrator      migrator-5db899446d-dlktq                                     10m (0%)      0 (0%)      200Mi (1%)       0 (0%)         6h5m
  openshift-machine-config-operator            machine-config-daemon-mk86p                                   40m (0%)      0 (0%)      100Mi (0%)       0 (0%)         6h5m
  openshift-machine-config-operator            machine-config-server-kdgrz                                   20m (0%)      0 (0%)      50Mi (0%)        0 (0%)         6h4m
  openshift-monitoring                         alertmanager-main-0                                           9m (0%)       0 (0%)      120Mi (0%)       0 (0%)         5h56m
  openshift-monitoring                         kube-state-metrics-84dd57c4df-nsgbh                           4m (0%)       0 (0%)      110Mi (0%)       0 (0%)         6h3m
  openshift-monitoring                         node-exporter-llscb                                           9m (0%)       0 (0%)      47Mi (0%)        0 (0%)         6h3m
  openshift-monitoring                         prometheus-k8s-0                                              75m (1%)      0 (0%)      1104Mi (7%)      0 (0%)         5h55m
  openshift-monitoring                         prometheus-operator-admission-webhook-58f7f489c4-dz9dg        5m (0%)       0 (0%)      30Mi (0%)        0 (0%)         6h3m
  openshift-monitoring                         telemeter-client-7cbbf7fb88-dh57v                             3m (0%)       0 (0%)      70Mi (0%)        0 (0%)         5h58m
  openshift-monitoring                         thanos-querier-d9484ff77-n4d86                                15m (0%)      0 (0%)      92Mi (0%)        0 (0%)         5h58m
  openshift-multus                             multus-6vkrs                                                  10m (0%)      0 (0%)      65Mi (0%)        0 (0%)         6h7m
  openshift-multus                             multus-additional-cni-plugins-t4rq8                           10m (0%)      0 (0%)      10Mi (0%)        0 (0%)         6h7m
  openshift-multus                             multus-admission-controller-6866f57bc6-rjgnq                  20m (0%)      0 (0%)      70Mi (0%)        0 (0%)         6h3m
  openshift-multus                             network-metrics-daemon-26dpr                                  20m (0%)      0 (0%)      120Mi (0%)       0 (0%)         6h7m
  openshift-network-diagnostics                network-check-target-2vrbr                                    10m (0%)      0 (0%)      15Mi (0%)        0 (0%)         6h6m
  openshift-oauth-apiserver                    apiserver-7d986f5684-ckrhd                                    150m (2%)     0 (0%)      200Mi (1%)       0 (0%)         5h56m
  openshift-operator-lifecycle-manager         packageserver-6f76c4df46-d8qf2                                10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         6h4m
  openshift-route-controller-manager           route-controller-manager-6585c8bb6b-znptx                     100m (1%)     0 (0%)      100Mi (0%)       0 (0%)         5h55m
  openshift-sdn                                sdn-controller-vrhjr                                          20m (0%)      0 (0%)      70Mi (0%)        0 (0%)         6h7m
  openshift-sdn                                sdn-t49t7                                                     110m (1%)     0 (0%)      220Mi (1%)       0 (0%)         6h7m
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests      Limits
  --------           --------      ------
  cpu                1940m (25%)   0 (0%)
  memory             7009Mi (48%)  0 (0%)
  ephemeral-storage  0 (0%)        0 (0%)
  hugepages-1Gi      0 (0%)        0 (0%)
  hugepages-2Mi      0 (0%)        0 (0%)
Events:
  Type    Reason          Age   From             Message
  ----    ------          ----  ----             -------
  Normal  RegisteredNode  51m   node-controller  Node master-1.ocp.tektutor.labs event: Registered Node master-1.ocp.tektutor.labs in Controller  
</pre>
