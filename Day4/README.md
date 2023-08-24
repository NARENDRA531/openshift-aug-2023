# Day 4

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
