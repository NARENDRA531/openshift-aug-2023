# Day 5

## Lab - Deploying an application using github repo

Deploying using docker strategy i.e using Dockerfile
```
oc new-project jegan
oc project jegan
oc new-app https://github.com/tektutor/spring-ms.git --strategy=docker
```

Checking the deployment status
```
oc status
oc logs -f bc/spring-ms 
oc expose svc/spring-ms
```

## Lab - Listing the buildconfigs
```
oc get buildconfigs
oc get buildconfig
oc get bc
```

## Lab - Listing the builds
```
oc get builds
oc get build
```

## Lab - Starting a build from buildconfig
```
oc start-build bc/spring-ms
oc logs -f bc/spring-ms
```

## Lab - Clean up all resources in your project
```
oc delete deploy/spring-ms svc/spring-ms bc/spring-ms is/spring-ms
```

## Lab - Deploying application using S2I(Source to Image) strategy
```
oc new-app registry.access.redhat.com/ubi8/openjdk-11~https://github.com/tektutor/spring-ms.git --strategy=source
```
In the above command, we have requested OpenShift to use registry.access.redhat.com/ubi8/openjdk-11 as a S2I image to perform the build and deploy it.

## Lab - Deploying an application using readily available Container Image from Docker Hub
```
oc new-app tektutor/spring-ms:1.0
```

Expected output
<pre>
(jegan@tektutor.org)$ oc new-app tektutor/spring-ms:1.0
--> Found container image 9175b94 (6 months old) from Docker Hub for "tektutor/spring-ms:1.0"

    * An image stream tag will be created as "spring-ms:1.0" that will track this image

--> Creating resources ...
    imagestream.image.openshift.io "spring-ms" created
Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "spring-ms" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "spring-ms" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "spring-ms" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "spring-ms" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
    deployment.apps "spring-ms" created
    service "spring-ms" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/spring-ms' 
    Run 'oc status' to view your app.
(jegan@tektutor.org)$ oc status
In project jegan on server https://api.ocp.tektutor.org:6443

http://spring-ms-jegan.apps.ocp.tektutor.org to pod port 8080-tcp (svc/spring-ms)
  deployment/spring-ms deploys istag/spring-ms:1.0 
    deployment #2 running for 3 seconds - 1 pod
    deployment #1 deployed 7 seconds ago

pod/example runs tektutor/spring-ms:1.0

1 info identified, use 'oc status --suggest' to see details.
</pre>



## Helm Overview
- Helm is a package manager for Kubernetes and OpenShift
- it is used to package our K8s/OpenShift applications as Helm Charts, install/uninstall or in general manage application installations within K8s/OpenShift

## Installing Helm3 in Linux
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```
When prompts for Administrator password, you may type 'rps@12345' without quotes.

## Lab - Parameterizing Wordpress/Mysql Helm charts
```
cd ~/openshift-aug-2023
git pull

cd Day4/helm/wordpress
helm package wordpress
helm install wp wordpress-0.1.0.tgz
helm list
```

## Lab - Deploying Red Hat JBoss AMQ 7 into Red Hat OpenShift Cluster

Note 
<pre>
- Only one person per OpenShift cluster can do this.
- Whoever is interested in installing the Operator, kindly leave a message via WebEx and let other participants know about the same to avoid conflicts, thanks!
</pre>

Let's begin
<pre>
1. Open the Red Hat OpenShift webconsole
2. As Administrator, click on Storage --> Persistent Volumes --> Create Persistent Volume and create your first PV as shown in the screenshot below  
3. As Administrator, click on Storage --> Persistent Volumes --> Create Persistent Volume and create your second PV as shown in the screenshot below     
4. As Administrator, Click on Operators --> Operator Hub, search for "AMQ Broker"
5. Select "Red Hat Integration - AMQ Broker for RHEL 8(Multi-arch)" and install accepting default options
6. Create an ActiveMQArtemises broker instance
</pre>
Step 1 - Open the Red Hat OpenShift webconsole
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/9d4116a0-9bc6-473b-8996-64f4a2a29a32)

Step 2 - As Administrator, click on Storage --> Persistent Volumes --> Create Persistent Volume and create your first PV as shown in the screenshot below  
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/242f0348-98f0-4fc8-8562-d259ad3dd9bb)


Step 3 - As Administrator, click on Storage --> Persistent Volumes --> Create Persistent Volume and create your second PV as shown in the screenshot below 
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/3972d4e7-a752-49d2-b60d-9eb043ff444b)


Step 4 - As Administrator, Click on Operators --> Operator Hub, search for "AMQ Broker"
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/d8b6e8bb-e4ef-4ef4-b5e4-be8eea78993b)

Step 5 - Select "Red Hat Integration - AMQ Broker for RHEL 8(Multi-arch)" and install accepting default options
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043
/99087804-0b03-4185-9058-050a3f4da81a)

Step 6 - Create an ActiveMQArtemises broker instance
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/d23e0375-0614-41a9-87a1-f22c37f9a428)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/81b25df8-23d0-4a03-82b6-fe622974b601)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/1baecc4b-ccf8-4ebd-bad4-2e479602dee9)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/001d66a1-2f3e-4c20-956c-4af842169c76)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/b76afad4-c453-4149-9c41-6c50d4f4e75f)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/f4d4e64c-0585-43e8-a07e-0f93beb1acb0)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/457ad9e0-85d9-49c9-b2c9-46a7b0c3a0be)




