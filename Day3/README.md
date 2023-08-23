# Day 3

## Deleting a service
```
oc delete svc/nginx
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc delete svc/nginx   
service "nginx" deleted  
</pre>

## Creating a NodePort external service
```
oc delete svc/nginx
oc expose deploy/nginx --type=NodePort --port=8080
oc get svc
oc describe svc/nginx
oc get endpoints
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~]
└─$ oc expose deploy/nginx --type=NodePort --port=8080                        
service/nginx exposed
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get svc                                        
NAME    TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
nginx   NodePort   172.30.249.211   <none>        8080:31006/TCP   3s
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc describe svc/nginx
Name:                     nginx
Namespace:                jegan
Labels:                   app=nginx
Annotations:              <none>
Selector:                 app=nginx
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       172.30.249.211
IPs:                      172.30.249.211
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  31006/TCP
Endpoints:                10.128.2.13:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
                                                                                                                
┌──(jegan㉿tektutor.org)-[~]
└─$ oc get endpoints     
NAME    ENDPOINTS          AGE
nginx   10.128.2.13:8080   8s
</pre>

## Demo - Installing MetalLB Operator to support LoadBalancer Service
You can follow the instructions from the below blog page
<pre>
https://medium.com/tektutor/using-metallb-loadbalancer-with-bare-metal-openshift-onprem-4230944bfa35  
</pre>


## Lab - Autogenerate nginx deployment manifest file to create deployment in declarative style
```
oc create deploy nginx --image=bitnami/nginx:latest --replicas=3 --dry-run=client
oc create deploy nginx --image=bitnami/nginx:latest --replicas=3 --dry-run=client -o yaml
ls
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day3/declarative]
└─$ oc create deploy nginx --image=bitnami/nginx:latest --replicas=3 --dry-run=client -o yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: bitnami/nginx:latest
        name: nginx
        resources: {}
status: {}
                                                                                                                                        
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day3/declarative]
└─$ oc create deploy nginx --image=bitnami/nginx:latest --replicas=3 --dry-run=client -o yaml > nginx-deploy.yml
                                                                                                                                        
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day3/declarative]
└─$ ls
nginx-deploy.yml  
</pre>
