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

## Red Hat OpenShift High-Level Architecture
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/28390b03-eb17-41ff-82ed-a520dbebac60)

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

## Lab - Listing pods from all namespaces
```
oc get pods --all-namespaces
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get pods --all-namespaces
NAMESPACE                                          NAME                                                         READY   STATUS      RESTARTS        AGE
openshift-apiserver-operator                       openshift-apiserver-operator-755cfb9b4c-5k7zl                1/1     Running     1 (6h45m ago)   6h50m
openshift-apiserver                                apiserver-59cbcb5597-29dth                                   2/2     Running     0               6h38m
openshift-apiserver                                apiserver-59cbcb5597-psqmf                                   2/2     Running     0               6h38m
openshift-apiserver                                apiserver-59cbcb5597-x8gsf                                   2/2     Running     0               6h37m
openshift-authentication-operator                  authentication-operator-567b547766-zbdcm                     1/1     Running     1 (6h45m ago)   6h50m
openshift-authentication                           oauth-openshift-7bc8477595-7m4mx                             1/1     Running     0               6h39m
openshift-authentication                           oauth-openshift-7bc8477595-dqqlb                             1/1     Running     0               6h39m
openshift-authentication                           oauth-openshift-7bc8477595-rl9mn                             1/1     Running     0               6h40m
openshift-cloud-controller-manager-operator        cluster-cloud-controller-manager-operator-7d85bfc559-nkhjn   2/2     Running     0               6h52m
openshift-cloud-credential-operator                cloud-credential-operator-8546b5744c-vvfvk                   2/2     Running     0               6h52m
openshift-cluster-machine-approver                 machine-approver-874c4b5b6-sqc7v                             2/2     Running     0               6h52m
openshift-cluster-node-tuning-operator             cluster-node-tuning-operator-f9876d97c-xmwrg                 1/1     Running     0               6h52m
openshift-cluster-node-tuning-operator             tuned-b4xl9                                                  1/1     Running     0               6h35m
openshift-cluster-node-tuning-operator             tuned-bxmm6                                                  1/1     Running     0               6h45m
openshift-cluster-node-tuning-operator             tuned-gcv8z                                                  1/1     Running     0               6h35m
openshift-cluster-node-tuning-operator             tuned-jwbd2                                                  1/1     Running     0               6h45m
openshift-cluster-node-tuning-operator             tuned-s85hm                                                  1/1     Running     0               6h45m
openshift-cluster-samples-operator                 cluster-samples-operator-7f97cd6698-fl277                    2/2     Running     1 (44m ago)     6h43m
openshift-cluster-storage-operator                 cluster-storage-operator-754d6f56b8-96w2m                    1/1     Running     1 (6h46m ago)   6h52m
openshift-cluster-storage-operator                 csi-snapshot-controller-9584854bf-m8gwq                      1/1     Running     0               6h47m
openshift-cluster-storage-operator                 csi-snapshot-controller-9584854bf-msfcb                      1/1     Running     0               6h47m
openshift-cluster-storage-operator                 csi-snapshot-controller-operator-7d99bff4d-cw9fn             1/1     Running     0               6h52m
openshift-cluster-storage-operator                 csi-snapshot-webhook-68694997c6-hpg5d                        1/1     Running     0               6h47m
openshift-cluster-storage-operator                 csi-snapshot-webhook-68694997c6-vff8l                        1/1     Running     0               6h47m
openshift-cluster-version                          cluster-version-operator-59d9468478-6smj9                    1/1     Running     0               6h52m
openshift-config-operator                          openshift-config-operator-7794fc8ccd-s545h                   1/1     Running     1 (6h46m ago)   6h52m
openshift-console-operator                         console-operator-75b68f4466-t984q                            2/2     Running     0               6h40m
openshift-console                                  console-d955f495-jnf82                                       1/1     Running     0               6h39m
openshift-console                                  console-d955f495-qj4ht                                       1/1     Running     0               6h39m
openshift-console                                  downloads-78b784cc4d-k49nx                                   1/1     Running     0               6h40m
openshift-console                                  downloads-78b784cc4d-wlzbd                                   1/1     Running     0               6h40m
openshift-controller-manager-operator              openshift-controller-manager-operator-589b88b589-77wlw       1/1     Running     1 (6h46m ago)   6h50m
openshift-controller-manager                       controller-manager-6c67dd645d-2k7h5                          1/1     Running     1 (92m ago)     6h38m
openshift-controller-manager                       controller-manager-6c67dd645d-h74w6                          1/1     Running     0               6h38m
openshift-controller-manager                       controller-manager-6c67dd645d-qmph7                          1/1     Running     0               6h38m
openshift-dns-operator                             dns-operator-58f6c6f864-p4rwc                                2/2     Running     0               6h50m
openshift-dns                                      dns-default-9tjgq                                            2/2     Running     0               6h46m
openshift-dns                                      dns-default-dfc29                                            2/2     Running     0               6h34m
openshift-dns                                      dns-default-fdd8j                                            2/2     Running     0               6h46m
openshift-dns                                      dns-default-g7xnk                                            2/2     Running     0               6h46m
openshift-dns                                      dns-default-k6b8m                                            2/2     Running     0               6h35m
openshift-dns                                      node-resolver-467bm                                          1/1     Running     0               6h46m
openshift-dns                                      node-resolver-4844r                                          1/1     Running     0               6h46m
openshift-dns                                      node-resolver-5jd7n                                          1/1     Running     0               6h35m
openshift-dns                                      node-resolver-6cwhx                                          1/1     Running     0               6h35m
openshift-dns                                      node-resolver-tt9ws                                          1/1     Running     0               6h46m
openshift-etcd-operator                            etcd-operator-74cc7479b7-8kd7d                               1/1     Running     1 (6h46m ago)   6h52m
openshift-etcd                                     etcd-guard-master-1.ocp.tektutor.labs                        1/1     Running     0               6h45m
openshift-etcd                                     etcd-guard-master-2.ocp.tektutor.labs                        1/1     Running     0               6h41m
openshift-etcd                                     etcd-guard-master-3.ocp.tektutor.labs                        1/1     Running     0               6h43m
openshift-etcd                                     etcd-master-1.ocp.tektutor.labs                              4/4     Running     0               6h33m
openshift-etcd                                     etcd-master-2.ocp.tektutor.labs                              4/4     Running     0               6h29m
openshift-etcd                                     etcd-master-3.ocp.tektutor.labs                              4/4     Running     0               6h31m
openshift-etcd                                     installer-4-master-3.ocp.tektutor.labs                       0/1     Completed   0               6h44m
openshift-etcd                                     installer-5-master-2.ocp.tektutor.labs                       0/1     Completed   0               6h42m
openshift-etcd                                     installer-6-master-1.ocp.tektutor.labs                       0/1     Completed   0               6h39m
openshift-etcd                                     installer-6-master-2.ocp.tektutor.labs                       0/1     Completed   0               6h41m
openshift-etcd                                     installer-6-master-3.ocp.tektutor.labs                       0/1     Completed   0               6h37m
openshift-etcd                                     installer-7-master-1.ocp.tektutor.labs                       0/1     Completed   0               6h34m
openshift-etcd                                     installer-7-master-2.ocp.tektutor.labs                       0/1     Completed   0               6h30m
openshift-etcd                                     installer-7-master-3.ocp.tektutor.labs                       0/1     Completed   0               6h32m
openshift-etcd                                     revision-pruner-6-master-1.ocp.tektutor.labs                 0/1     Completed   0               6h35m
openshift-etcd                                     revision-pruner-6-master-2.ocp.tektutor.labs                 0/1     Completed   0               6h35m
openshift-etcd                                     revision-pruner-6-master-3.ocp.tektutor.labs                 0/1     Completed   0               6h35m
openshift-etcd                                     revision-pruner-7-master-1.ocp.tektutor.labs                 0/1     Completed   0               6h35m
openshift-etcd                                     revision-pruner-7-master-2.ocp.tektutor.labs                 0/1     Completed   0               6h35m
openshift-etcd                                     revision-pruner-7-master-3.ocp.tektutor.labs                 0/1     Completed   0               6h35m
openshift-image-registry                           cluster-image-registry-operator-5c8d659cc5-dgrmz             1/1     Running     0               6h52m
openshift-image-registry                           image-registry-5c648d4b4b-wbs9b                              1/1     Running     0               6h38m
openshift-image-registry                           node-ca-87mfx                                                1/1     Running     0               6h38m
openshift-image-registry                           node-ca-cxrl7                                                1/1     Running     0               6h38m
openshift-image-registry                           node-ca-gcgpz                                                1/1     Running     0               6h35m
openshift-image-registry                           node-ca-lhfqp                                                1/1     Running     0               6h38m
openshift-image-registry                           node-ca-mgp6x                                                1/1     Running     0               6h35m
openshift-ingress-canary                           ingress-canary-4vrkq                                         1/1     Running     0               6h40m
openshift-ingress-canary                           ingress-canary-7l6g9                                         1/1     Running     0               6h40m
openshift-ingress-canary                           ingress-canary-9kpbc                                         1/1     Running     0               6h35m
openshift-ingress-canary                           ingress-canary-jbpm6                                         1/1     Running     0               6h40m
openshift-ingress-canary                           ingress-canary-ws52m                                         1/1     Running     0               6h34m
openshift-ingress-operator                         ingress-operator-fbd867bd4-xvw74                             2/2     Running     2 (6h41m ago)   6h52m
openshift-ingress                                  router-default-6b9d4d5bfd-6s2nk                              1/1     Running     0               6h36m
openshift-ingress                                  router-default-6b9d4d5bfd-jcjbl                              1/1     Running     0               6h40m
openshift-ingress                                  router-default-6b9d4d5bfd-mcw2j                              1/1     Running     0               6h38m
openshift-insights                                 insights-operator-bcc49f96d-gzjhw                            1/1     Running     1 (6h46m ago)   6h52m
openshift-kube-apiserver-operator                  kube-apiserver-operator-66c5497cc8-zv2bj                     1/1     Running     1 (6h46m ago)   6h52m
openshift-kube-apiserver                           installer-10-master-1.ocp.tektutor.labs                      0/1     Completed   0               6h35m
openshift-kube-apiserver                           installer-10-master-2.ocp.tektutor.labs                      0/1     Completed   0               6h38m
openshift-kube-apiserver                           installer-10-master-3.ocp.tektutor.labs                      0/1     Completed   0               6h33m
openshift-kube-apiserver                           installer-9-master-2.ocp.tektutor.labs                       0/1     Completed   0               6h39m
openshift-kube-apiserver                           kube-apiserver-guard-master-1.ocp.tektutor.labs              1/1     Running     0               6h34m
openshift-kube-apiserver                           kube-apiserver-guard-master-2.ocp.tektutor.labs              1/1     Running     0               6h39m
openshift-kube-apiserver                           kube-apiserver-guard-master-3.ocp.tektutor.labs              1/1     Running     0               6h43m
openshift-kube-apiserver                           kube-apiserver-master-1.ocp.tektutor.labs                    5/5     Running     0               6h34m
openshift-kube-apiserver                           kube-apiserver-master-2.ocp.tektutor.labs                    5/5     Running     1 (92m ago)     6h36m
openshift-kube-apiserver                           kube-apiserver-master-3.ocp.tektutor.labs                    5/5     Running     0               6h31m
openshift-kube-apiserver                           revision-pruner-10-master-1.ocp.tektutor.labs                0/1     Completed   0               6h31m
openshift-kube-apiserver                           revision-pruner-10-master-2.ocp.tektutor.labs                0/1     Completed   0               6h31m
openshift-kube-apiserver                           revision-pruner-10-master-3.ocp.tektutor.labs                0/1     Completed   0               6h31m
openshift-kube-controller-manager-operator         kube-controller-manager-operator-b6fdddd6f-ww2q8             1/1     Running     1 (6h46m ago)   6h50m
openshift-kube-controller-manager                  installer-4-master-1.ocp.tektutor.labs                       0/1     Completed   0               6h45m
openshift-kube-controller-manager                  installer-4-master-3.ocp.tektutor.labs                       0/1     Completed   0               6h43m
openshift-kube-controller-manager                  installer-5-master-1.ocp.tektutor.labs                       0/1     Completed   0               6h40m
openshift-kube-controller-manager                  installer-5-master-2.ocp.tektutor.labs                       0/1     Completed   0               6h41m
openshift-kube-controller-manager                  installer-5-master-3.ocp.tektutor.labs                       0/1     Completed   0               6h38m
openshift-kube-controller-manager                  installer-6-master-1.ocp.tektutor.labs                       0/1     Completed   0               6h35m
openshift-kube-controller-manager                  installer-6-master-2.ocp.tektutor.labs                       0/1     Completed   0               6h33m
openshift-kube-controller-manager                  installer-6-master-3.ocp.tektutor.labs                       0/1     Completed   0               6h37m
openshift-kube-controller-manager                  kube-controller-manager-guard-master-1.ocp.tektutor.labs     1/1     Running     0               6h44m
openshift-kube-controller-manager                  kube-controller-manager-guard-master-2.ocp.tektutor.labs     1/1     Running     0               6h40m
openshift-kube-controller-manager                  kube-controller-manager-guard-master-3.ocp.tektutor.labs     1/1     Running     0               6h42m
openshift-kube-controller-manager                  kube-controller-manager-master-1.ocp.tektutor.labs           4/4     Running     0               6h34m
openshift-kube-controller-manager                  kube-controller-manager-master-2.ocp.tektutor.labs           4/4     Running     1 (92m ago)     6h32m
openshift-kube-controller-manager                  kube-controller-manager-master-3.ocp.tektutor.labs           4/4     Running     2 (93m ago)     6h35m
openshift-kube-controller-manager                  revision-pruner-6-master-1.ocp.tektutor.labs                 0/1     Completed   0               6h32m
openshift-kube-controller-manager                  revision-pruner-6-master-2.ocp.tektutor.labs                 0/1     Completed   0               6h32m
openshift-kube-controller-manager                  revision-pruner-6-master-3.ocp.tektutor.labs                 0/1     Completed   0               6h32m
openshift-kube-scheduler-operator                  openshift-kube-scheduler-operator-7c8cb5446-6r5tb            1/1     Running     1 (6h46m ago)   6h50m
openshift-kube-scheduler                           installer-5-master-2.ocp.tektutor.labs                       0/1     Completed   0               6h46m
openshift-kube-scheduler                           installer-7-master-1.ocp.tektutor.labs                       0/1     Completed   0               6h43m
openshift-kube-scheduler                           installer-7-master-2.ocp.tektutor.labs                       0/1     Completed   0               6h45m
openshift-kube-scheduler                           installer-8-master-1.ocp.tektutor.labs                       0/1     Completed   0               6h41m
openshift-kube-scheduler                           installer-8-master-2.ocp.tektutor.labs                       0/1     Completed   0               6h38m
openshift-kube-scheduler                           installer-8-master-3.ocp.tektutor.labs                       0/1     Completed   0               6h39m
openshift-kube-scheduler                           openshift-kube-scheduler-guard-master-1.ocp.tektutor.labs    1/1     Running     0               6h42m
openshift-kube-scheduler                           openshift-kube-scheduler-guard-master-2.ocp.tektutor.labs    1/1     Running     0               6h45m
openshift-kube-scheduler                           openshift-kube-scheduler-guard-master-3.ocp.tektutor.labs    1/1     Running     0               6h39m
openshift-kube-scheduler                           openshift-kube-scheduler-master-1.ocp.tektutor.labs          3/3     Running     0               6h40m
openshift-kube-scheduler                           openshift-kube-scheduler-master-2.ocp.tektutor.labs          3/3     Running     0               6h37m
openshift-kube-scheduler                           openshift-kube-scheduler-master-3.ocp.tektutor.labs          3/3     Running     0               6h39m
openshift-kube-scheduler                           revision-pruner-6-master-1.ocp.tektutor.labs                 0/1     Completed   0               6h45m
openshift-kube-scheduler                           revision-pruner-6-master-2.ocp.tektutor.labs                 0/1     Completed   0               6h45m
openshift-kube-scheduler                           revision-pruner-6-master-3.ocp.tektutor.labs                 0/1     Completed   0               6h45m
openshift-kube-scheduler                           revision-pruner-7-master-1.ocp.tektutor.labs                 0/1     Completed   0               6h45m
openshift-kube-scheduler                           revision-pruner-7-master-2.ocp.tektutor.labs                 0/1     Completed   0               6h45m
openshift-kube-scheduler                           revision-pruner-7-master-3.ocp.tektutor.labs                 0/1     Completed   0               6h45m
openshift-kube-scheduler                           revision-pruner-8-master-1.ocp.tektutor.labs                 0/1     Completed   0               6h41m
openshift-kube-scheduler                           revision-pruner-8-master-2.ocp.tektutor.labs                 0/1     Completed   0               6h42m
openshift-kube-scheduler                           revision-pruner-8-master-3.ocp.tektutor.labs                 0/1     Completed   0               6h41m
openshift-kube-storage-version-migrator-operator   kube-storage-version-migrator-operator-5886f9d9c4-t7w6t      1/1     Running     1 (6h46m ago)   6h50m
openshift-kube-storage-version-migrator            migrator-5db899446d-dlktq                                    1/1     Running     0               6h47m
openshift-machine-api                              cluster-autoscaler-operator-64c6945d46-n5q5v                 2/2     Running     0               6h52m
openshift-machine-api                              cluster-baremetal-operator-67cd754774-7m8pn                  2/2     Running     0               6h52m
openshift-machine-api                              control-plane-machine-set-operator-584746b655-lz6nr          1/1     Running     0               6h52m
openshift-machine-api                              machine-api-operator-7474957984-whc4w                        2/2     Running     0               6h52m
openshift-machine-config-operator                  machine-config-controller-767846fc4c-h6pqm                   2/2     Running     0               6h46m
openshift-machine-config-operator                  machine-config-daemon-56j79                                  2/2     Running     0               6h35m
openshift-machine-config-operator                  machine-config-daemon-gdj5f                                  2/2     Running     0               6h35m
openshift-machine-config-operator                  machine-config-daemon-hzj55                                  2/2     Running     0               6h48m
openshift-machine-config-operator                  machine-config-daemon-jdstr                                  2/2     Running     0               6h48m
openshift-machine-config-operator                  machine-config-daemon-mk86p                                  2/2     Running     0               6h48m
openshift-machine-config-operator                  machine-config-operator-7987b6bcdd-6g22v                     1/1     Running     0               6h52m
openshift-machine-config-operator                  machine-config-server-bt466                                  1/1     Running     0               6h46m
openshift-machine-config-operator                  machine-config-server-fn82r                                  1/1     Running     0               6h46m
openshift-machine-config-operator                  machine-config-server-kdgrz                                  1/1     Running     0               6h46m
openshift-marketplace                              certified-operators-xc498                                    1/1     Running     0               6h45m
openshift-marketplace                              community-operators-gzw54                                    1/1     Running     0               6h45m
openshift-marketplace                              marketplace-operator-6dcf8f6556-db682                        1/1     Running     0               6h50m
openshift-marketplace                              redhat-marketplace-r8qlx                                     1/1     Running     0               6h45m
openshift-marketplace                              redhat-operators-62rb4                                       1/1     Running     0               6h45m
openshift-monitoring                               alertmanager-main-0                                          6/6     Running     0               6h38m
openshift-monitoring                               alertmanager-main-1                                          6/6     Running     1 (6h39m ago)   6h39m
openshift-monitoring                               cluster-monitoring-operator-7bd79c884d-9z5qb                 1/1     Running     0               6h52m
openshift-monitoring                               kube-state-metrics-84dd57c4df-nsgbh                          3/3     Running     0               6h45m
openshift-monitoring                               node-exporter-4szs5                                          2/2     Running     0               6h45m
openshift-monitoring                               node-exporter-7fllg                                          2/2     Running     0               6h35m
openshift-monitoring                               node-exporter-llscb                                          2/2     Running     0               6h45m
openshift-monitoring                               node-exporter-s6b8k                                          2/2     Running     0               6h35m
openshift-monitoring                               node-exporter-v5mrz                                          2/2     Running     0               6h45m
openshift-monitoring                               openshift-state-metrics-87485d769-k5dlb                      3/3     Running     0               6h45m
openshift-monitoring                               prometheus-adapter-6f67697675-lfjl6                          1/1     Running     0               6h40m
openshift-monitoring                               prometheus-adapter-6f67697675-stmpt                          1/1     Running     0               6h40m
openshift-monitoring                               prometheus-k8s-0                                             6/6     Running     0               6h38m
openshift-monitoring                               prometheus-k8s-1                                             6/6     Running     0               6h39m
openshift-monitoring                               prometheus-operator-5c59544657-mfqdq                         2/2     Running     0               6h46m
openshift-monitoring                               prometheus-operator-admission-webhook-58f7f489c4-dz9dg       1/1     Running     0               6h46m
openshift-monitoring                               prometheus-operator-admission-webhook-58f7f489c4-pxhbs       1/1     Running     0               6h46m
openshift-monitoring                               telemeter-client-7cbbf7fb88-dh57v                            3/3     Running     0               6h40m
openshift-monitoring                               thanos-querier-d9484ff77-n4d86                               6/6     Running     0               6h40m
openshift-monitoring                               thanos-querier-d9484ff77-w5rm4                               6/6     Running     0               6h40m
openshift-multus                                   multus-4sk85                                                 1/1     Running     0               6h35m
openshift-multus                                   multus-4vbf4                                                 1/1     Running     0               6h49m
openshift-multus                                   multus-6vkrs                                                 1/1     Running     0               6h49m
openshift-multus                                   multus-7jwmk                                                 1/1     Running     0               6h49m
openshift-multus                                   multus-additional-cni-plugins-qj8j8                          1/1     Running     0               6h49m
openshift-multus                                   multus-additional-cni-plugins-rs5xt                          1/1     Running     0               6h49m
openshift-multus                                   multus-additional-cni-plugins-t4rq8                          1/1     Running     0               6h49m
openshift-multus                                   multus-additional-cni-plugins-wxfck                          1/1     Running     0               6h35m
openshift-multus                                   multus-additional-cni-plugins-x2x2t                          1/1     Running     0               6h35m
openshift-multus                                   multus-admission-controller-6866f57bc6-f5qdz                 2/2     Running     0               6h45m
openshift-multus                                   multus-admission-controller-6866f57bc6-rjgnq                 2/2     Running     0               6h45m
openshift-multus                                   multus-qhlws                                                 1/1     Running     0               6h35m
openshift-multus                                   network-metrics-daemon-26dpr                                 2/2     Running     0               6h49m
openshift-multus                                   network-metrics-daemon-999n5                                 2/2     Running     0               6h35m
openshift-multus                                   network-metrics-daemon-dznpr                                 2/2     Running     0               6h49m
openshift-multus                                   network-metrics-daemon-rhz2j                                 2/2     Running     0               6h35m
openshift-multus                                   network-metrics-daemon-xfbjl                                 2/2     Running     0               6h49m
openshift-network-diagnostics                      network-check-source-7f8d955c9d-ntp6d                        1/1     Running     0               6h49m
openshift-network-diagnostics                      network-check-target-2vrbr                                   1/1     Running     0               6h49m
openshift-network-diagnostics                      network-check-target-4d25m                                   1/1     Running     0               6h35m
openshift-network-diagnostics                      network-check-target-7w994                                   1/1     Running     0               6h49m
openshift-network-diagnostics                      network-check-target-ccmlr                                   1/1     Running     0               6h35m
openshift-network-diagnostics                      network-check-target-gzwwf                                   1/1     Running     0               6h49m
openshift-network-operator                         network-operator-58794dbd64-bsw9g                            1/1     Running     1 (6h46m ago)   6h50m
openshift-oauth-apiserver                          apiserver-7d986f5684-6dspk                                   1/1     Running     0               6h40m
openshift-oauth-apiserver                          apiserver-7d986f5684-ckrhd                                   1/1     Running     0               6h39m
openshift-oauth-apiserver                          apiserver-7d986f5684-lhxdv                                   1/1     Running     0               6h41m
openshift-operator-lifecycle-manager               catalog-operator-74567567b6-pmnwf                            1/1     Running     0               6h52m
openshift-operator-lifecycle-manager               collect-profiles-28211610-5lgp9                              0/1     Completed   0               35m
openshift-operator-lifecycle-manager               collect-profiles-28211625-rpp7m                              0/1     Completed   0               20m
openshift-operator-lifecycle-manager               collect-profiles-28211640-n8h6r                              0/1     Completed   0               5m18s
openshift-operator-lifecycle-manager               olm-operator-69ff6c9444-b47dq                                1/1     Running     0               6h52m
openshift-operator-lifecycle-manager               package-server-manager-6b9ccb565f-b7xg5                      1/1     Running     0               6h52m
openshift-operator-lifecycle-manager               packageserver-6f76c4df46-d8qf2                               1/1     Running     0               6h46m
openshift-operator-lifecycle-manager               packageserver-6f76c4df46-sf9fk                               1/1     Running     0               6h46m
openshift-route-controller-manager                 route-controller-manager-6585c8bb6b-9k9nh                    1/1     Running     0               6h38m
openshift-route-controller-manager                 route-controller-manager-6585c8bb6b-mwtjn                    1/1     Running     0               6h38m
openshift-route-controller-manager                 route-controller-manager-6585c8bb6b-znptx                    1/1     Running     0               6h38m
openshift-sdn                                      sdn-controller-h4stv                                         2/2     Running     0               6h49m
openshift-sdn                                      sdn-controller-vrhjr                                         2/2     Running     1 (92m ago)     6h49m
openshift-sdn                                      sdn-controller-vwj8d                                         2/2     Running     0               6h49m
openshift-sdn                                      sdn-nx4wb                                                    2/2     Running     0               6h35m
openshift-sdn                                      sdn-qs25t                                                    2/2     Running     0               6h49m
openshift-sdn                                      sdn-r2fmg                                                    2/2     Running     0               6h49m
openshift-sdn                                      sdn-t49t7                                                    2/2     Running     0               6h49m
openshift-sdn                                      sdn-xjw2h                                                    2/2     Running     0               6h35m
openshift-service-ca-operator                      service-ca-operator-7bd94cf7b5-fxxss                         1/1     Running     1 (6h46m ago)   6h50m
openshift-service-ca                               service-ca-857cbb9846-jjzrj                                  1/1     Running     0               6h47m  
</pre>

## Lab - Listing all namespaces or projects
```
oc get namespaces
oc get projects
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get namespaces  
NAME                                               STATUS   AGE
default                                            Active   6h59m
kube-node-lease                                    Active   6h59m
kube-public                                        Active   6h59m
kube-system                                        Active   6h59m
openshift                                          Active   6h46m
openshift-apiserver                                Active   6h49m
openshift-apiserver-operator                       Active   6h58m
openshift-authentication                           Active   6h49m
openshift-authentication-operator                  Active   6h58m
openshift-cloud-controller-manager                 Active   6h58m
openshift-cloud-controller-manager-operator        Active   6h58m
openshift-cloud-credential-operator                Active   6h58m
openshift-cloud-network-config-controller          Active   6h58m
openshift-cluster-csi-drivers                      Active   6h58m
openshift-cluster-machine-approver                 Active   6h58m
openshift-cluster-node-tuning-operator             Active   6h58m
openshift-cluster-samples-operator                 Active   6h58m
openshift-cluster-storage-operator                 Active   6h58m
openshift-cluster-version                          Active   6h59m
openshift-config                                   Active   6h58m
openshift-config-managed                           Active   6h58m
openshift-config-operator                          Active   6h58m
openshift-console                                  Active   6h43m
openshift-console-operator                         Active   6h43m
openshift-console-user-settings                    Active   6h43m
openshift-controller-manager                       Active   6h50m
openshift-controller-manager-operator              Active   6h58m
openshift-dns                                      Active   6h48m
openshift-dns-operator                             Active   6h58m
openshift-etcd                                     Active   6h58m
openshift-etcd-operator                            Active   6h58m
openshift-host-network                             Active   6h52m
openshift-image-registry                           Active   6h58m
openshift-infra                                    Active   6h59m
openshift-ingress                                  Active   6h48m
openshift-ingress-canary                           Active   6h43m
openshift-ingress-operator                         Active   6h58m
openshift-insights                                 Active   6h58m
openshift-kni-infra                                Active   6h58m
openshift-kube-apiserver                           Active   6h58m
openshift-kube-apiserver-operator                  Active   6h58m
openshift-kube-controller-manager                  Active   6h58m
openshift-kube-controller-manager-operator         Active   6h58m
openshift-kube-scheduler                           Active   6h58m
openshift-kube-scheduler-operator                  Active   6h58m
openshift-kube-storage-version-migrator            Active   6h50m
openshift-kube-storage-version-migrator-operator   Active   6h58m
openshift-machine-api                              Active   6h58m
openshift-machine-config-operator                  Active   6h58m
openshift-marketplace                              Active   6h58m
openshift-monitoring                               Active   6h58m
openshift-multus                                   Active   6h52m
openshift-network-diagnostics                      Active   6h52m
openshift-network-operator                         Active   6h58m
openshift-node                                     Active   6h46m
openshift-nutanix-infra                            Active   6h58m
openshift-oauth-apiserver                          Active   6h49m
openshift-openstack-infra                          Active   6h58m
openshift-operator-lifecycle-manager               Active   6h58m
openshift-operators                                Active   6h58m
openshift-ovirt-infra                              Active   6h58m
openshift-route-controller-manager                 Active   6h50m
openshift-sdn                                      Active   6h52m
openshift-service-ca                               Active   6h50m
openshift-service-ca-operator                      Active   6h58m
openshift-user-workload-monitoring                 Active   6h58m
openshift-vsphere-infra                            Active   6h58m
                                                                                                                                        
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get projects  
NAME                                               DISPLAY NAME   STATUS
default                                                           Active
kube-node-lease                                                   Active
kube-public                                                       Active
kube-system                                                       Active
openshift                                                         Active
openshift-apiserver                                               Active
openshift-apiserver-operator                                      Active
openshift-authentication                                          Active
openshift-authentication-operator                                 Active
openshift-cloud-controller-manager                                Active
openshift-cloud-controller-manager-operator                       Active
openshift-cloud-credential-operator                               Active
openshift-cloud-network-config-controller                         Active
openshift-cluster-csi-drivers                                     Active
openshift-cluster-machine-approver                                Active
openshift-cluster-node-tuning-operator                            Active
openshift-cluster-samples-operator                                Active
openshift-cluster-storage-operator                                Active
openshift-cluster-version                                         Active
openshift-config                                                  Active
openshift-config-managed                                          Active
openshift-config-operator                                         Active
openshift-console                                                 Active
openshift-console-operator                                        Active
openshift-console-user-settings                                   Active
openshift-controller-manager                                      Active
openshift-controller-manager-operator                             Active
openshift-dns                                                     Active
openshift-dns-operator                                            Active
openshift-etcd                                                    Active
openshift-etcd-operator                                           Active
openshift-host-network                                            Active
openshift-image-registry                                          Active
openshift-infra                                                   Active
openshift-ingress                                                 Active
openshift-ingress-canary                                          Active
openshift-ingress-operator                                        Active
openshift-insights                                                Active
openshift-kni-infra                                               Active
openshift-kube-apiserver                                          Active
openshift-kube-apiserver-operator                                 Active
openshift-kube-controller-manager                                 Active
openshift-kube-controller-manager-operator                        Active
openshift-kube-scheduler                                          Active
openshift-kube-scheduler-operator                                 Active
openshift-kube-storage-version-migrator                           Active
openshift-kube-storage-version-migrator-operator                  Active
openshift-machine-api                                             Active
openshift-machine-config-operator                                 Active
openshift-marketplace                                             Active
openshift-monitoring                                              Active
openshift-multus                                                  Active
openshift-network-diagnostics                                     Active
openshift-network-operator                                        Active
openshift-node                                                    Active
openshift-nutanix-infra                                           Active
openshift-oauth-apiserver                                         Active
openshift-openstack-infra                                         Active
openshift-operator-lifecycle-manager                              Active
openshift-operators                                               Active
openshift-ovirt-infra                                             Active
openshift-route-controller-manager                                Active
openshift-sdn                                                     Active
openshift-service-ca                                              Active
openshift-service-ca-operator                                     Active
openshift-user-workload-monitoring                                Active
openshift-vsphere-infra                                           Active  
</pre>

## Lab - Listing all the Custom Resource Definitions (CRDs)
```
oc get crds
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get crds                                
NAME                                                              CREATED AT
alertmanagerconfigs.monitoring.coreos.com                         2023-08-22T03:09:11Z
alertmanagers.monitoring.coreos.com                               2023-08-22T03:09:14Z
apirequestcounts.apiserver.openshift.io                           2023-08-22T03:08:55Z
apiservers.config.openshift.io                                    2023-08-22T03:08:32Z
authentications.config.openshift.io                               2023-08-22T03:08:32Z
authentications.operator.openshift.io                             2023-08-22T03:09:13Z
baremetalhosts.metal3.io                                          2023-08-22T03:09:12Z
bmceventsubscriptions.metal3.io                                   2023-08-22T03:09:14Z
builds.config.openshift.io                                        2023-08-22T03:08:33Z
catalogsources.operators.coreos.com                               2023-08-22T03:09:12Z
cloudcredentials.operator.openshift.io                            2023-08-22T03:08:56Z
clusterautoscalers.autoscaling.openshift.io                       2023-08-22T03:09:10Z
clustercsidrivers.operator.openshift.io                           2023-08-22T03:09:51Z
clusternetworks.network.openshift.io                              2023-08-22T03:15:40Z
clusteroperators.config.openshift.io                              2023-08-22T03:08:20Z
clusterresourcequotas.quota.openshift.io                          2023-08-22T03:08:31Z
clusterserviceversions.operators.coreos.com                       2023-08-22T03:09:14Z
clusterversions.config.openshift.io                               2023-08-22T03:08:20Z
configs.imageregistry.operator.openshift.io                       2023-08-22T03:09:10Z
configs.operator.openshift.io                                     2023-08-22T03:09:16Z
configs.samples.operator.openshift.io                             2023-08-22T03:09:08Z
consoleclidownloads.console.openshift.io                          2023-08-22T03:09:08Z
consoleexternalloglinks.console.openshift.io                      2023-08-22T03:09:08Z
consolelinks.console.openshift.io                                 2023-08-22T03:09:08Z
consolenotifications.console.openshift.io                         2023-08-22T03:09:08Z
consoleplugins.console.openshift.io                               2023-08-22T03:09:08Z
consolequickstarts.console.openshift.io                           2023-08-22T03:09:08Z
consoles.config.openshift.io                                      2023-08-22T03:08:33Z
consoles.operator.openshift.io                                    2023-08-22T03:09:10Z
consoleyamlsamples.console.openshift.io                           2023-08-22T03:09:08Z
containerruntimeconfigs.machineconfiguration.openshift.io         2023-08-22T03:09:32Z
controllerconfigs.machineconfiguration.openshift.io               2023-08-22T03:17:15Z
controlplanemachinesets.machine.openshift.io                      2023-08-22T03:09:12Z
credentialsrequests.cloudcredential.openshift.io                  2023-08-22T03:08:55Z
csisnapshotcontrollers.operator.openshift.io                      2023-08-22T03:09:10Z
dnses.config.openshift.io                                         2023-08-22T03:08:33Z
dnses.operator.openshift.io                                       2023-08-22T03:09:16Z
dnsrecords.ingress.operator.openshift.io                          2023-08-22T03:09:14Z
egressnetworkpolicies.network.openshift.io                        2023-08-22T03:15:40Z
egressrouters.network.operator.openshift.io                       2023-08-22T03:09:21Z
etcds.operator.openshift.io                                       2023-08-22T03:09:08Z
featuregates.config.openshift.io                                  2023-08-22T03:08:34Z
firmwareschemas.metal3.io                                         2023-08-22T03:09:17Z
hardwaredata.metal3.io                                            2023-08-22T03:09:20Z
helmchartrepositories.helm.openshift.io                           2023-08-22T03:09:08Z
hostfirmwaresettings.metal3.io                                    2023-08-22T03:09:21Z
hostsubnets.network.openshift.io                                  2023-08-22T03:15:40Z
imagecontentpolicies.config.openshift.io                          2023-08-22T03:08:35Z
imagecontentsourcepolicies.operator.openshift.io                  2023-08-22T03:08:35Z
imagedigestmirrorsets.config.openshift.io                         2023-08-22T03:08:35Z
imagepruners.imageregistry.operator.openshift.io                  2023-08-22T03:09:43Z
images.config.openshift.io                                        2023-08-22T03:08:34Z
imagetagmirrorsets.config.openshift.io                            2023-08-22T03:08:36Z
infrastructures.config.openshift.io                               2023-08-22T03:08:36Z
ingresscontrollers.operator.openshift.io                          2023-08-22T03:08:59Z
ingresses.config.openshift.io                                     2023-08-22T03:08:37Z
insightsoperators.operator.openshift.io                           2023-08-22T03:24:46Z
installplans.operators.coreos.com                                 2023-08-22T03:09:17Z
ippools.whereabouts.cni.cncf.io                                   2023-08-22T03:15:34Z
kubeapiservers.operator.openshift.io                              2023-08-22T03:09:46Z
kubecontrollermanagers.operator.openshift.io                      2023-08-22T03:09:13Z
kubeletconfigs.machineconfiguration.openshift.io                  2023-08-22T03:09:34Z
kubeschedulers.operator.openshift.io                              2023-08-22T03:09:14Z
kubestorageversionmigrators.operator.openshift.io                 2023-08-22T03:09:08Z
machineautoscalers.autoscaling.openshift.io                       2023-08-22T03:09:13Z
machineconfigpools.machineconfiguration.openshift.io              2023-08-22T03:09:38Z
machineconfigs.machineconfiguration.openshift.io                  2023-08-22T03:09:36Z
machinehealthchecks.machine.openshift.io                          2023-08-22T03:09:51Z
machines.machine.openshift.io                                     2023-08-22T03:09:49Z
machinesets.machine.openshift.io                                  2023-08-22T03:09:51Z
metal3remediations.infrastructure.cluster.x-k8s.io                2023-08-22T03:09:51Z
metal3remediationtemplates.infrastructure.cluster.x-k8s.io        2023-08-22T03:09:53Z
netnamespaces.network.openshift.io                                2023-08-22T03:15:40Z
network-attachment-definitions.k8s.cni.cncf.io                    2023-08-22T03:15:34Z
networks.config.openshift.io                                      2023-08-22T03:08:37Z
networks.operator.openshift.io                                    2023-08-22T03:09:13Z
nodes.config.openshift.io                                         2023-08-22T03:08:37Z
oauths.config.openshift.io                                        2023-08-22T03:08:38Z
olmconfigs.operators.coreos.com                                   2023-08-22T03:09:24Z
openshiftapiservers.operator.openshift.io                         2023-08-22T03:09:08Z
openshiftcontrollermanagers.operator.openshift.io                 2023-08-22T03:09:13Z
operatorconditions.operators.coreos.com                           2023-08-22T03:09:27Z
operatorgroups.operators.coreos.com                               2023-08-22T03:09:28Z
operatorhubs.config.openshift.io                                  2023-08-22T03:09:08Z
operatorpkis.network.operator.openshift.io                        2023-08-22T03:09:23Z
operators.operators.coreos.com                                    2023-08-22T03:09:31Z
overlappingrangeipreservations.whereabouts.cni.cncf.io            2023-08-22T03:15:34Z
performanceprofiles.performance.openshift.io                      2023-08-22T03:09:13Z
podmonitors.monitoring.coreos.com                                 2023-08-22T03:09:17Z
podnetworkconnectivitychecks.controlplane.operator.openshift.io   2023-08-22T03:34:08Z
preprovisioningimages.metal3.io                                   2023-08-22T03:09:24Z
probes.monitoring.coreos.com                                      2023-08-22T03:09:20Z
profiles.tuned.openshift.io                                       2023-08-22T03:09:16Z
projecthelmchartrepositories.helm.openshift.io  
projects.config.openshift.io                                      2023-08-22T03:08:38Z
prometheuses.monitoring.coreos.com                                2023-08-22T03:09:21Z
prometheusrules.monitoring.coreos.com                             2023-08-22T03:09:24Z
provisionings.metal3.io                                           2023-08-22T03:10:16Z
proxies.config.openshift.io                                       2023-08-22T03:08:31Z
rangeallocations.security.internal.openshift.io                   2023-08-22T03:08:31Z
rolebindingrestrictions.authorization.openshift.io                2023-08-22T03:08:31Z
schedulers.config.openshift.io                                    2023-08-22T03:08:39Z
securitycontextconstraints.security.openshift.io                  2023-08-22T03:08:31Z
servicecas.operator.openshift.io                                  2023-08-22T03:09:17Z
servicemonitors.monitoring.coreos.com                             2023-08-22T03:09:27Z
storages.operator.openshift.io                                    2023-08-22T03:09:51Z
storagestates.migration.k8s.io                                    2023-08-22T03:09:13Z
storageversionmigrations.migration.k8s.io                         2023-08-22T03:09:10Z
subscriptions.operators.coreos.com                                2023-08-22T03:09:46Z
thanosrulers.monitoring.coreos.com                                2023-08-22T03:09:28Z
tuneds.tuned.openshift.io                                         2023-08-22T03:09:18Z
volumesnapshotclasses.snapshot.storage.k8s.io                     2023-08-22T03:17:45Z
volumesnapshotcontents.snapshot.storage.k8s.io                    2023-08-22T03:17:45Z
volumesnapshots.snapshot.storage.k8s.io                           2023-08-22T03:17:45Z  
</pre>


## Lab - Finding help about any OpenShift resource
```
oc explain deploy
```

Expected output
```
──(jegan㉿tektutor.org)-[~]
└─$ oc explain deploy
KIND:     Deployment
VERSION:  apps/v1

DESCRIPTION:
     Deployment enables declarative updates for Pods and ReplicaSets.

FIELDS:
   apiVersion	<string>
     APIVersion defines the versioned schema of this representation of an
     object. Servers should convert recognized schemas to the latest internal
     value, and may reject unrecognized values. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources

   kind	<string>
     Kind is a string value representing the REST resource this object
     represents. Servers may infer this from the endpoint the client submits
     requests to. Cannot be updated. In CamelCase. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

   metadata	<Object>
     Standard object's metadata. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata

   spec	<Object>
     Specification of the desired behavior of the Deployment.

   status	<Object>
     Most recently observed status of the Deployment.  
```


## Lab - Creating a project to deployment your applications
```
oc new-project jegan
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc new-project jegan
Now using project "jegan" on server "https://api.ocp.tektutor.labs:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname  
</pre>

## Lab - Checking your current project
```
oc project
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc project          
Using project "jegan" on server "https://api.ocp.tektutor.labs:6443".  
</pre>

## Lab - Creating your first deployment in Red Hat OpenShift
```
oc create deployment nginx --image=nginx:latest
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc create deployment nginx --image=nginx:latest
deployment.apps/nginx created
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get deploy,rs,po                            
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   0/1     1            0           13s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-654975c8cd   1         1         0       13s

NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-654975c8cd-rh7ws   0/1     ContainerCreating   0          13s
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get po -w       
NAME                     READY   STATUS             RESTARTS     AGE
nginx-654975c8cd-rh7ws   0/1     CrashLoopBackOff   1 (9s ago)   30s
nginx-654975c8cd-rh7ws   0/1     Error              2 (19s ago)   40s
nginx-654975c8cd-rh7ws   0/1     CrashLoopBackOff   2 (12s ago)   51s
^C                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc logs nginx-654975c8cd-rh7ws
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: can not modify /etc/nginx/conf.d/default.conf (read-only file system?)
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/08/22 10:30:52 [warn] 1#1: the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
nginx: [warn] the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
2023/08/22 10:30:52 [emerg] 1#1: mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
nginx: [emerg] mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get po -w                  
NAME                     READY   STATUS             RESTARTS      AGE
nginx-654975c8cd-rh7ws   0/1     CrashLoopBackOff   4 (25s ago)   2m27s  
</pre>

## Lab - Deleting a deployment
```
oc delete deploy/nginx
oc get deploy,rs,po
```

Expected output
<pre>                   
┌──(jegan㉿tektutor.org)-[~]
└─$ oc delete deploy/nginx                              
deployment.apps "nginx" deleted
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get deploy,rs,po
No resources found in jegan namespace.  
</pre>

## Lab - Creating a nginx deployment using bitnami nginx container image
```
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc create deployment nginx --image=bitnami/nginx:latest
deployment.apps/nginx created
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get deploy                                          
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/1     1            0           4s
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get rs    
NAME               DESIRED   CURRENT   READY   AGE
nginx-5bccb79775   1         1         0       7s
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get po   
NAME                     READY   STATUS              RESTARTS   AGE
nginx-5bccb79775-xdnm6   0/1     ContainerCreating   0          9s
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get po -w
NAME                     READY   STATUS              RESTARTS   AGE
nginx-5bccb79775-xdnm6   0/1     ContainerCreating   0          12s
nginx-5bccb79775-xdnm6   1/1     Running             0          14s
^C                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc logs nginx-5bccb79775-xdnm6
nginx 10:40:35.76 
nginx 10:40:35.76 Welcome to the Bitnami nginx container
nginx 10:40:35.77 Subscribe to project updates by watching https://github.com/bitnami/containers
nginx 10:40:35.77 Submit issues and feature requests at https://github.com/bitnami/containers/issues
nginx 10:40:35.77 
nginx 10:40:35.77 INFO  ==> ** Starting NGINX setup **
nginx 10:40:35.79 INFO  ==> Validating settings in NGINX_* env vars
Generating RSA private key, 4096 bit long modulus (2 primes)
.............................................................++++
...............................................++++
e is 65537 (0x010001)
Signature ok
subject=CN = example.com
Getting Private key
nginx 10:40:36.35 INFO  ==> No custom scripts in /docker-entrypoint-initdb.d
nginx 10:40:36.36 INFO  ==> Initializing NGINX
realpath: /bitnami/nginx/conf/vhosts: No such file or directory
nginx 10:40:36.41 INFO  ==> ** NGINX setup finished! **

nginx 10:40:36.44 INFO  ==> ** Starting NGINX **  
</pre>

## Lab - Scaling up/down deployment
```
oc create deployment nginx --image=bitnami/nginx:latest
oc get deploy
oc get rs
oc get po
oc scale deploy/nginx --replicas=5
oc get po
oc scale deploy/nginx --replicas=3
oc get po
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc create deployment nginx --image=bitnami/nginx:latest                                      
deployment.apps/nginx created
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get deploy                                          
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/1     1            0           5s
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get rs    
NAME               DESIRED   CURRENT   READY   AGE
nginx-5bccb79775   1         1         1       8s
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get po
NAME                     READY   STATUS    RESTARTS   AGE
nginx-5bccb79775-69qhd   1/1     Running   0          10s
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc scale deploy/nginx --replicas=5
deployment.apps/nginx scaled
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get po -w                      
NAME                     READY   STATUS              RESTARTS   AGE
nginx-5bccb79775-69qhd   1/1     Running             0          20s
nginx-5bccb79775-bcmw9   0/1     ContainerCreating   0          3s
nginx-5bccb79775-f8g9s   0/1     ContainerCreating   0          3s
nginx-5bccb79775-lk9xv   0/1     ContainerCreating   0          3s
nginx-5bccb79775-tr74k   0/1     ContainerCreating   0          3s
nginx-5bccb79775-tr74k   1/1     Running             0          5s
nginx-5bccb79775-f8g9s   1/1     Running             0          5s
nginx-5bccb79775-lk9xv   1/1     Running             0          6s
nginx-5bccb79775-bcmw9   1/1     Running             0          6s
^C                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get po   
NAME                     READY   STATUS    RESTARTS   AGE
nginx-5bccb79775-69qhd   1/1     Running   0          27s
nginx-5bccb79775-bcmw9   1/1     Running   0          10s
nginx-5bccb79775-f8g9s   1/1     Running   0          10s
nginx-5bccb79775-lk9xv   1/1     Running   0          10s
nginx-5bccb79775-tr74k   1/1     Running   0          10s
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc scale deploy/nginx --replicas=3
deployment.apps/nginx scaled
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get po                         
NAME                     READY   STATUS        RESTARTS   AGE
nginx-5bccb79775-69qhd   1/1     Running       0          47s
nginx-5bccb79775-bcmw9   1/1     Running       0          30s
nginx-5bccb79775-lk9xv   1/1     Running       0          30s
nginx-5bccb79775-tr74k   1/1     Terminating   0          30s
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get po
NAME                     READY   STATUS    RESTARTS   AGE
nginx-5bccb79775-69qhd   1/1     Running   0          50s
nginx-5bccb79775-bcmw9   1/1     Running   0          33s
nginx-5bccb79775-lk9xv   1/1     Running   0          33s  
</pre>

## Lab - Setting up a port forward for testing/debugging Pod
```
oc port-forward pod/nginx-5bccb79775-lk9xv 8080:8080
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc port-forward pod/nginx-5bccb79775-lk9xv 8080:8080
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080  
</pre>

You may now access the web page from your CentOS lab web browser
<pre>
http://localhost:8080  
</pre>
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/163102cf-e2d5-480f-9daf-75ec6e327278)

## Kubernetes/OpenShift Services
There are 3 types of Services 
1. ClusterIP (Internal Service)
2. NodePort (External Service)
3. LoadBalancer (External Service meant to be used in Public Cloud Environment)

## Lab - Creating an Internal ClusterIP service for nginx deployment
```
oc expose deploy/nginx --type=ClusterIP --port=8080
oc get svc
oc describe svc/nginx
oc get pods -o wide --show-labels
oc get endpoints
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc expose deploy/nginx --type=ClusterIP --port=8080                 
service/nginx exposed
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get svc                                         
NAME    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
nginx   ClusterIP   172.30.243.159   <none>        8080/TCP   4s
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc describe svc/nginx          
Name:              nginx
Namespace:         jegan
Labels:            app=nginx
Annotations:       <none>
Selector:          app=nginx
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                172.30.243.159
IPs:               172.30.243.159
Port:              <unset>  8080/TCP
TargetPort:        8080/TCP
Endpoints:         10.128.2.13:8080
Session Affinity:  None
Events:            <none>
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get po -o wide --show-labels
NAME                     READY   STATUS    RESTARTS   AGE   IP            NODE                         NOMINATED NODE   READINESS GATES   LABELS
nginx-5bccb79775-f9zfk   1/1     Running   0          73s   10.128.2.13   worker-1.ocp.tektutor.labs   <none>           <none>            app=nginx,pod-template-hash=5bccb79775  

┌──(jegan㉿tektutor.org)-[~]
└─$ oc get endpoints               
NAME    ENDPOINTS          AGE
nginx   10.128.2.13:8080   2m12s  
</pre>
