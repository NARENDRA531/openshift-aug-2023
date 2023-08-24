# Day 4

## Multi-stage Dockerfile
```
https://github.com/tektutor/spring-ms.git
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
