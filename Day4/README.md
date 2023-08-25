# Day 4

## Cloning TekTutor Trainig Repo
```
cd ~
git clone https://github.com/tektutor/openshift-aug-2023
```

## Lab - Installing Helm Package Manager in your RPS CentOS Lab Machine
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

helm version
```
Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm]
└─$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
Downloading https://get.helm.sh/helm-v3.12.3-linux-amd64.tar.gz
Verifying checksum... Done.
Preparing to install helm into /usr/local/bin
[sudo] password for jegan: 
helm installed into /usr/local/bin/helm  

┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm]
└─$ helm version  
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/jegan/.kube/config
version.BuildInfo{Version:"v3.12.3", GitCommit:"3a31588ad33fe3b89af5a2a54ee1d25bfe6eaa5e", GitTreeState:"clean", GoVersion:"go1.20.7"}  
</pre>


## Lab - Creating a helm chart that uses Parameters
```
cd ~/openshift-aug-2023
git pull
cd Day4/helm
rm wordpress-0.1.0.tgz
helm package wordpress
ls
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm]
└─$ pwd
/home/jegan/openshift-aug-2023/Day4/helm
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm]
└─$ helm package wordpress
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/jegan/.kube/config
Successfully packaged chart and saved it to: /home/jegan/openshift-aug-2023/Day4/helm/wordpress-0.1.0.tgz
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm]
└─$ ls
wordpress  wordpress-0.1.0.tgz
</pre>

## Lab - Install our custom wordpress helm chart
```
cd ~/openshift-aug-2023
git pull
cd Day4/helm
ls
helm install wordpress wordpress-0.1.0.tgz
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm]
└─$ ls
wordpress  wordpress-0.1.0.tgz
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm]
└─$ helm install wordpress wordpress-0.1.0.tgz  
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/jegan/.kube/config
NAME: wordpress
LAST DEPLOYED: Fri Aug 25 11:24:37 2023
NAMESPACE: jegan
STATUS: deployed
REVISION: 1
TEST SUITE: None  
</pre>

## Lab - Helm wordpress upgrade
You need to add some to the wordpress-deploy.yml and mysql-deploy.yml and bump up the Chart.yaml version from 0.1.0 to 0.1.1 before proceeding with the below commands.

```
cd ~/openshift-aug-2023
git pull
cd Day4/helm
ls
helm package wordpress
ls
helm upgrade wordpress wordpress-0.1.1.tgz
helm list 
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm]
└─$ helm package wordpress                    
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/jegan/.kube/config
Successfully packaged chart and saved it to: /home/jegan/openshift-aug-2023/Day4/helm/wordpress-0.1.1.tgz
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm]
└─$ ls
wordpress  wordpress-0.1.0.tgz  wordpress-0.1.1.tgz
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm]
└─$ helm list             
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/jegan/.kube/config
NAME     	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART          	APP VERSION
wordpress	jegan    	1       	2023-08-25 11:24:37.308055202 +0530 IST	deployed	wordpress-0.1.0	1.16.0     
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm]
└─$ helm upgrade wordpress wordpress-0.1.1.tgz
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/jegan/.kube/config
Release "wordpress" has been upgraded. Happy Helming!
NAME: wordpress
LAST DEPLOYED: Fri Aug 25 11:28:58 2023
NAMESPACE: jegan
STATUS: deployed
REVISION: 2
TEST SUITE: None
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm]
└─$ helm list                                 
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/jegan/.kube/config
NAME     	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART          	APP VERSION
wordpress	jegan    	2       	2023-08-25 11:28:58.668274502 +0530 IST	deployed	wordpress-0.1.1	1.16.0       
</pre>

## Lab - Rolling back to previous version of wordpress using Helm
```
helm list
helm rollback wordpress
helm list
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm/wordpress]
└─$ helm list                  
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/jegan/.kube/config
NAME     	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART          	APP VERSION
wordpress	jegan    	2       	2023-08-25 11:28:58.668274502 +0530 IST	deployed	wordpress-0.1.1	1.16.0     
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm/wordpress]
└─$ helm rollback wordpress    
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/jegan/.kube/config
Rollback was a success! Happy Helming!
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day4/helm/wordpress]
└─$ helm list              
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/jegan/.kube/config
NAME     	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART          	APP VERSION
wordpress	jegan    	3       	2023-08-25 11:33:56.906038023 +0530 IST	deployed	wordpress-0.1.0	1.16.0       
</pre>

## Lab - Understanding service discovery

Service discovery helps us access a service by its name within the Kubernetes/OpenShift cluster.

Let's us list the services
```
oc new-
oc get svc
oc describe svc/nginx
```
To test the service discovery, let's create a utility pod

oc create deploy hello --image=tektutor/spring-tektutor-helloms:latest

Let's get inside the hello pod

oc rsh deploy/hello

Let's see if we are able to access the nginx service by its name within the hello pod

curl http://nginx:8080

In the above url, nginx is the name of NodePort service. You are expected to see response from the nginx pod.

You may also check the /etc/resolv.conf content within the hello pod

cat /etc/resolv.conf

You will see a nameserver IP, that nameserver is the one which resolves the nginx service name to its corresponding IP address.

## Multi-stage Dockerfile
```
https://github.com/tektutor/spring-ms.git
```

## Setting up NFS Server
```
su -
yum install -y nfs-utils
systemctl enable nfs-server
systemctl start nfs-server
systemctl status nfs-server
```

Let's create nfs shared folder in Server 1 (OpenShift Cluster 1 - 10.10.15.85)
```
su -
mkdir -p /mnt/nfs/user01
mkdir -p /mnt/nfs/user02
mkdir -p /mnt/nfs/user03
mkdir -p /mnt/nfs/user04
mkdir -p /mnt/nfs/user05
mkdir -p /mnt/nfs/user06
mkdir -p /mnt/nfs/user07

chmod 755 -R /mnt/nfs
chown nfsnobody:nfsnobody -R /mnt/nfs

systemctl enable rpcbind
systemctl enable nfs-server
systemctl enable nfs-lock
systemctl enable nfs-idmap
systemctl start rpcbind
systemctl start nfs-server
systemctl start nfs-lock
systemctl start nfs-idmap
exportfs -r
```

Let's create nfs shared folder in Server 2 (OpenShift Cluster 2 - 10.10.15.99)
```
su -
mkdir -p /mnt/nfs/user08
mkdir -p /mnt/nfs/user09
mkdir -p /mnt/nfs/user10
mkdir -p /mnt/nfs/user11
mkdir -p /mnt/nfs/user12
mkdir -p /mnt/nfs/user13
mkdir -p /mnt/nfs/user14
mkdir -p /mnt/nfs/user15
mkdir -p /mnt/nfs/user16

chmod 755 -R /mnt/nfs
chown nfsnobody:nfsnobody -R /mnt/nfs

systemctl enable rpcbind
systemctl enable nfs-server
systemctl enable nfs-lock
systemctl enable nfs-idmap
systemctl start rpcbind
systemctl start nfs-server
systemctl start nfs-lock
systemctl start nfs-idmap
exportfs -r
```

Let's create nfs shared folder in Server 3 (OpenShift Cluster 3 - 10.10.15.101)
```
su -
mkdir -p /mnt/nfs/user17
mkdir -p /mnt/nfs/user18
mkdir -p /mnt/nfs/user19
mkdir -p /mnt/nfs/user20
mkdir -p /mnt/nfs/user21
mkdir -p /mnt/nfs/user22
mkdir -p /mnt/nfs/user23
mkdir -p /mnt/nfs/user24
mkdir -p /mnt/nfs/user25
chmod 755 -R /mnt/nfs
chown nfsnobody:nfsnobody -R /mnt/nfs

systemctl enable rpcbind
systemctl enable nfs-server
systemctl enable nfs-lock
systemctl enable nfs-idmap
systemctl start rpcbind
systemctl start nfs-server
systemctl start nfs-lock
systemctl start nfs-idmap
exportfs -r
```



Create a file /etc/exports file with the below values on Server 1 (10.10.15.85)
```
/mnt/nfs/user01				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user02				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user03				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user04				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user05				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user06				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user07				192.168.122.0/24(rw,sync,no_root_squash)
```

Create a file /etc/exports file with the below values on Server 2 (10.10.15.99)
```
/mnt/nfs/user08				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user09				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user10				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user11				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user12				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user13				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user14				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user15				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user16				192.168.122.0/24(rw,sync,no_root_squash)
```

Create a file /etc/exports file with the below values on Server 3 (10.10.15.101)
```
/mnt/nfs/user17				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user18				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user19				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user20				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user21				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user22				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user23				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user24				192.168.122.0/24(rw,sync,no_root_squash)
/mnt/nfs/user25				192.168.122.0/24(rw,sync,no_root_squash)
```
Let's open up the firewall ports to expose the NFS folder to our Openshift cluster virtual machines
```
su -
firewall-cmd --permanent --add-service=nfs
firewall-cmd --permanent --add-service=rpc-bind
firewall-cmd --permanent --add-service=mountd
firewall-cmd --reload
```

Testing if the NFS Server setup works
```
su -
showmount -e
```

## Lab - Creating a route to expose the service to the external world
```
oc create deploy nginx --image=bitnami/nginx --replicas=3
oc get deploy
oc get po
oc expose deploy/nginx --type=ClusterIP--port=8080
oc expose svc/nginx
oc get route
```

You may now try accessing the route from outside the cluster as shown below
<pre>
http://nginx-jegan.apps.ocp.rps.com
</pre>

## Lab - Deploying a multi-pod application in declarative style
In this lab exercise we will learning the following topics
1. How to make use PersistentVolume ( external storage ) in OpenShift application workloads
2. How to deploy a multi-pod application
3. ConfigMap
4. Secrets

The application is Wordpress which depends on MySQL database.

This exercise involves deployment wordpress separately and mysql separately.

Let's first create the mysql deployment
```
cd ~/openshift-aug-2023
git pull
cd Day4/wordpress-configmap-and-secrets/
oc apply -f mysql-pv.yml
oc apply -f mysql-pvc.yml
oc apply -f mysql-deploy.yml
oc apply -f mysql-svc.yml
```

Let's deploy wordpress now
```
cd ~/openshift-aug-2023
git pull
cd Day4/wordpress-configmap-and-secrets/
oc apply -f wordpress-pv.yml
oc apply -f wordpress-pvc.yml
oc apply -f wordpress-deploy.yml
oc apply -f wordpress-svc.yml
oc apply -f wordpress-route.yml
```

You may now check if the mysql and wordpress pods are in running state
```
oc get po 
```

You may also check if the mysql pod is ready to accept user connections
```
oc logs -f mysql-886b485d4-5j5g5
```

You may also check if the wordpress pod is ready
```
oc logs -f wordpress-6b6554f4f6-whpjx
```

Once you are sure that both mysql and wordpress is all set, you may find the route url
```
oc get route
```

You can head over to your CentOS chrome browser and navigate to route url
```
http://wordpress-jegan.apps.ocp.tektutor.labs
```
