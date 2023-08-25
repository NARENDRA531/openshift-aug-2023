# Day 5

## Ingress
You may find this blog interesting
<pre>
https://medium.com/@jegan_50867/redhat-openshift-ingress-e91f27a35773   
</pre>

There are critical components
1. Ingress rule ( user-defined )
2. Ingress Controller
3. Load Balancer ( nginx or haproxy )

You need to update the base url as per the output you see for the below command
```
oc describe ingresscontroller/default -n openshift-ingress-operator | grep Domain:
```
Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ingress]
└─$ oc describe ingresscontroller/default -n openshift-ingress-operator | grep Domain:
  Domain:                  apps.ocp.tektutor.labs   
</pre>

```
cd ~
cd openshift-may-2023
cd Day5/ingress

oc create deploy nginx --image=bitnami/nginx:latest --replicas=3
oc expose deploy/nginx --port=8080

oc create deploy hello --image=tektutor/spring-tektutor-helloms:latest --replicas=3
oc expose deploy/hello --port=8080

oc apply -f ingress.yml
oc get ingress
oc describe ingress tektutor
```

Let's test if the ingress works as expected
```
jegan@tektutor:~/openshift-may-2023/Day5/ingress$ curl tektutor.apps.ocp4.tektutor.org.ocp/nginx
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

jegan@tektutor:~/openshift-may-2023/Day5/ingress$ curl tektutor.apps.ocp4.tektutor.org.ocp/hello
Hello Java Microsrvice !
```

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

Step 1 - Open the Red Hat OpenShift webconsole
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/9d4116a0-9bc6-473b-8996-64f4a2a29a32)

Step 2 - As Administrator, click on Storage --> Persistent Volumes --> Create Persistent Volume and create your first PV as shown in the screenshot below  
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/242f0348-98f0-4fc8-8562-d259ad3dd9bb)

Step 3 - As Administrator, click on Storage --> Persistent Volumes --> Create Persistent Volume and create your second PV as shown in the screenshot below 
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/3972d4e7-a752-49d2-b60d-9eb043ff444b)

Step 4 - As Administrator, Click on Operators --> Operator Hub, search for "AMQ Broker"
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/d8b6e8bb-e4ef-4ef4-b5e4-be8eea78993b)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/8cdc57a5-e679-479a-88a7-891da318c43d)


Step 5 - Select "Red Hat Integration - AMQ Broker for RHEL 8(Multi-arch)" and install accepting default options
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/fa071491-f021-4249-8f73-b83eb8955d28)

Step 6 - Create an ActiveMQArtemises broker instance
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/d23e0375-0614-41a9-87a1-f22c37f9a428)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/81b25df8-23d0-4a03-82b6-fe622974b601)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/1baecc4b-ccf8-4ebd-bad4-2e479602dee9)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/001d66a1-2f3e-4c20-956c-4af842169c76)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/b76afad4-c453-4149-9c41-6c50d4f4e75f)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/40beef9e-8ba0-4d38-aef9-36b2799f6b8b)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/457ad9e0-85d9-49c9-b2c9-46a7b0c3a0be)

Step 7 - Create a route for AMQ Broker
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/86ee935f-03df-4cda-8b24-164c414e6ec3)

Step 8 - Accessing Red Hat AMQ Management Console from browser
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/7155613f-b2bb-4c9b-bcad-83cd0f4a93ba)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/74faf6ef-1ef7-4377-8cc8-5eba3c500ee1)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/89c50286-10ae-4646-9f8a-bb345c8ac45a)

Step 9 - Retrieving Login Credentials
![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/b2af90bb-1781-49df-b19d-7b6d2d896df9)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/b3cf5c41-b118-406e-9f9b-0893f28762de)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/b031f0d8-839b-4b26-a7e7-280e29c21318)

Step 10 - Login to Red Hat AMQ Management Console using
AMQ_USER and AMQ_PASSWORD from the secret

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/58fdcb96-a995-4034-a760-7b0de2cb7cbf)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/d4830c6c-5bf0-4f28-b5ba-acf49be4b83f)

![image](https://github.com/tektutor/openshift-aug-2023/assets/12674043/71a30d6c-b62e-4196-be11-94ececa57500)

<pre>
amq-broker/bin/artemis producer --user admin --password admin --url tcp://ex-aao-ss-0:61616 --message-count 100
  
amq-broker/bin/artemis consumer --user admin --password admin --url tcp://ex-aao-ss-1:61616 --message-count 100

  
</pre>


## Demo - Building a custom Ansible node image
```
cd ~/openshift-aug-2023
git pull
cd Day3/ubuntu-ansible
docker build -t tektutor/ansible-ubuntu-node:latest .
```

Expected output
<pre>
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ ssh-keygen       
Generating public/private rsa key pair.
Enter file in which to save the key (/home/jegan/.ssh/id_rsa): 
Created directory '/home/jegan/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/jegan/.ssh/id_rsa
Your public key has been saved in /home/jegan/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:pSMfWXlhpLTxHOhaDxDOwJvAjweyMSF8U/kqPTv5/pA jegan@tektutor
The key's randomart image is:
+---[RSA 3072]----+
|o .o.oo ..oo=    |
| o+o+..+...O o   |
|  .=.=.ooo* +    |
|  . . =. =+.     |
|    ..o So o     |
|   . + ooo  .    |
|    . +E.        |
|     +  .        |
|      +o..       |
+----[SHA256]-----+
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ ls
Dockerfile
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ cp /home/jegan/.ssh/id_rsa.pub authorized_keys
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ ls
authorized_keys  Dockerfile
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ vim Dockerfile 
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ ls
authorized_keys  Dockerfile
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ docker build -t tektutor/ansible-ubuntu-node:latest .
Sending build context to Docker daemon  4.096kB
Step 1/12 : FROM ubuntu:16.04
16.04: Pulling from library/ubuntu
58690f9b18fc: Pull complete 
b51569e7c507: Pull complete 
da8ef40b9eca: Pull complete 
fb15d46c38dc: Pull complete 
Digest: sha256:1f1a2d56de1d604801a9671f301190704c25d604a416f59e03c04f5c6ffee0d6
Status: Downloaded newer image for ubuntu:16.04
 ---> b6f507652425
Step 2/12 : MAINTAINER Jeganathan Swaminathan <jegan@tektutor.org>
 ---> Running in d3963ae4c0a4
Removing intermediate container d3963ae4c0a4
 ---> 9fd051c56afe
Step 3/12 : RUN apt-get update && apt-get install -y openssh-server python3
 ---> Running in 91f90eae75ef
Get:1 http://security.ubuntu.com/ubuntu xenial-security InRelease [99.8 kB]
Get:2 http://archive.ubuntu.com/ubuntu xenial InRelease [247 kB]
Get:3 http://security.ubuntu.com/ubuntu xenial-security/main amd64 Packages [2052 kB]
Get:4 http://archive.ubuntu.com/ubuntu xenial-updates InRelease [99.8 kB]
Get:5 http://archive.ubuntu.com/ubuntu xenial-backports InRelease [97.4 kB]
Get:6 http://archive.ubuntu.com/ubuntu xenial/main amd64 Packages [1558 kB]
Get:7 http://security.ubuntu.com/ubuntu xenial-security/restricted amd64 Packages [15.9 kB]
Get:8 http://security.ubuntu.com/ubuntu xenial-security/universe amd64 Packages [985 kB]
Get:9 http://security.ubuntu.com/ubuntu xenial-security/multiverse amd64 Packages [8820 B]
Get:10 http://archive.ubuntu.com/ubuntu xenial/restricted amd64 Packages [14.1 kB]
Get:11 http://archive.ubuntu.com/ubuntu xenial/universe amd64 Packages [9827 kB]
Get:12 http://archive.ubuntu.com/ubuntu xenial/multiverse amd64 Packages [176 kB]
Get:13 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages [2560 kB]
Get:14 http://archive.ubuntu.com/ubuntu xenial-updates/restricted amd64 Packages [16.4 kB]
Get:15 http://archive.ubuntu.com/ubuntu xenial-updates/universe amd64 Packages [1545 kB]
Get:16 http://archive.ubuntu.com/ubuntu xenial-updates/multiverse amd64 Packages [25.0 kB]
Get:17 http://archive.ubuntu.com/ubuntu xenial-backports/main amd64 Packages [10.9 kB]
Get:18 http://archive.ubuntu.com/ubuntu xenial-backports/universe amd64 Packages [12.7 kB]
Fetched 19.4 MB in 10s (1867 kB/s)
Reading package lists...
Reading package lists...
Building dependency tree...
Reading state information...
The following additional packages will be installed:
  ca-certificates dh-python file krb5-locales libbsd0 libedit2 libexpat1
  libgssapi-krb5-2 libidn11 libk5crypto3 libkeyutils1 libkrb5-3
  libkrb5support0 libmagic1 libmpdec2 libpython3-stdlib libpython3.5-minimal
  libpython3.5-stdlib libsqlite3-0 libssl1.0.0 libwrap0 libx11-6 libx11-data
  libxau6 libxcb1 libxdmcp6 libxext6 libxmuu1 mime-support ncurses-term
  openssh-client openssh-sftp-server openssl python3-chardet python3-minimal
  python3-pkg-resources python3-requests python3-six python3-urllib3 python3.5
  python3.5-minimal ssh-import-id tcpd wget xauth
Suggested packages:
  libdpkg-perl krb5-doc krb5-user ssh-askpass libpam-ssh keychain monkeysphere
  rssh molly-guard ufw python3-doc python3-tk python3-venv python3-setuptools
  python3-ndg-httpsclient python3-openssl python3-pyasn1 python3.5-venv
  python3.5-doc binutils binfmt-support
The following NEW packages will be installed:
  ca-certificates dh-python file krb5-locales libbsd0 libedit2 libexpat1
  libgssapi-krb5-2 libidn11 libk5crypto3 libkeyutils1 libkrb5-3
  libkrb5support0 libmagic1 libmpdec2 libpython3-stdlib libpython3.5-minimal
  libpython3.5-stdlib libsqlite3-0 libssl1.0.0 libwrap0 libx11-6 libx11-data
  libxau6 libxcb1 libxdmcp6 libxext6 libxmuu1 mime-support ncurses-term
  openssh-client openssh-server openssh-sftp-server openssl python3
  python3-chardet python3-minimal python3-pkg-resources python3-requests
  python3-six python3-urllib3 python3.5 python3.5-minimal ssh-import-id tcpd
  wget xauth
0 upgraded, 47 newly installed, 0 to remove and 1 not upgraded.
Need to get 10.5 MB of archives.
After this operation, 55.0 MB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libssl1.0.0 amd64 1.0.2g-1ubuntu4.20 [1083 kB]
Get:2 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libpython3.5-minimal amd64 3.5.2-2ubuntu0~16.04.13 [524 kB]
Get:3 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libexpat1 amd64 2.1.0-7ubuntu0.16.04.5 [71.5 kB]
Get:4 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 python3.5-minimal amd64 3.5.2-2ubuntu0~16.04.13 [1597 kB]
Get:5 http://archive.ubuntu.com/ubuntu xenial/main amd64 python3-minimal amd64 3.5.1-3 [23.3 kB]
Get:6 http://archive.ubuntu.com/ubuntu xenial/main amd64 mime-support all 3.59ubuntu1 [31.0 kB]
Get:7 http://archive.ubuntu.com/ubuntu xenial/main amd64 libmpdec2 amd64 2.4.2-1 [82.6 kB]
Get:8 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libsqlite3-0 amd64 3.11.0-1ubuntu1.5 [398 kB]
Get:9 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libpython3.5-stdlib amd64 3.5.2-2ubuntu0~16.04.13 [2135 kB]
Get:10 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 python3.5 amd64 3.5.2-2ubuntu0~16.04.13 [165 kB]
Get:11 http://archive.ubuntu.com/ubuntu xenial/main amd64 libpython3-stdlib amd64 3.5.1-3 [6818 B]
Get:12 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 dh-python all 2.20151103ubuntu1.2 [73.9 kB]
Get:13 http://archive.ubuntu.com/ubuntu xenial/main amd64 python3 amd64 3.5.1-3 [8710 B]
Get:14 http://archive.ubuntu.com/ubuntu xenial/main amd64 libxau6 amd64 1:1.0.8-1 [8376 B]
Get:15 http://archive.ubuntu.com/ubuntu xenial/main amd64 libxdmcp6 amd64 1:1.1.2-1.1 [11.0 kB]
Get:16 http://archive.ubuntu.com/ubuntu xenial/main amd64 libxcb1 amd64 1.11.1-1ubuntu1 [40.0 kB]
Get:17 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libx11-data all 2:1.6.3-1ubuntu2.2 [114 kB]
Get:18 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libx11-6 amd64 2:1.6.3-1ubuntu2.2 [572 kB]
Get:19 http://archive.ubuntu.com/ubuntu xenial/main amd64 libxext6 amd64 2:1.3.3-1 [29.4 kB]
Get:20 http://archive.ubuntu.com/ubuntu xenial/main amd64 libwrap0 amd64 7.6.q-25 [46.2 kB]
Get:21 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libmagic1 amd64 1:5.25-2ubuntu1.4 [216 kB]
Get:22 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 file amd64 1:5.25-2ubuntu1.4 [21.2 kB]
Get:23 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libbsd0 amd64 0.8.2-1ubuntu0.1 [42.0 kB]
Get:24 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libidn11 amd64 1.32-3ubuntu1.2 [46.5 kB]
Get:25 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 openssl amd64 1.0.2g-1ubuntu4.20 [492 kB]
Get:26 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 ca-certificates all 20210119~16.04.1 [148 kB]
Get:27 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 krb5-locales all 1.13.2+dfsg-5ubuntu2.2 [13.7 kB]
Get:28 http://archive.ubuntu.com/ubuntu xenial/main amd64 libedit2 amd64 3.1-20150325-1ubuntu2 [76.5 kB]
Get:29 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libkrb5support0 amd64 1.13.2+dfsg-5ubuntu2.2 [31.2 kB]
Get:30 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libk5crypto3 amd64 1.13.2+dfsg-5ubuntu2.2 [81.2 kB]
Get:31 http://archive.ubuntu.com/ubuntu xenial/main amd64 libkeyutils1 amd64 1.5.9-8ubuntu1 [9904 B]
Get:32 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libkrb5-3 amd64 1.13.2+dfsg-5ubuntu2.2 [273 kB]
Get:33 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libgssapi-krb5-2 amd64 1.13.2+dfsg-5ubuntu2.2 [120 kB]
Get:34 http://archive.ubuntu.com/ubuntu xenial/main amd64 libxmuu1 amd64 2:1.1.2-2 [9674 B]
Get:35 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 openssh-client amd64 1:7.2p2-4ubuntu2.10 [590 kB]
Get:36 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 wget amd64 1.17.1-1ubuntu1.5 [299 kB]
Get:37 http://archive.ubuntu.com/ubuntu xenial/main amd64 xauth amd64 1:1.0.9-1ubuntu2 [22.7 kB]
Get:38 http://archive.ubuntu.com/ubuntu xenial/main amd64 ncurses-term all 6.0+20160213-1ubuntu1 [249 kB]
Get:39 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 openssh-sftp-server amd64 1:7.2p2-4ubuntu2.10 [38.8 kB]
Get:40 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 openssh-server amd64 1:7.2p2-4ubuntu2.10 [335 kB]
Get:41 http://archive.ubuntu.com/ubuntu xenial/main amd64 python3-pkg-resources all 20.7.0-1 [79.0 kB]
Get:42 http://archive.ubuntu.com/ubuntu xenial/main amd64 python3-chardet all 2.3.0-2 [96.2 kB]
Get:43 http://archive.ubuntu.com/ubuntu xenial/main amd64 python3-six all 1.10.0-3 [11.0 kB]
Get:44 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 python3-urllib3 all 1.13.1-2ubuntu0.16.04.4 [58.6 kB]
Get:45 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 python3-requests all 2.9.1-3ubuntu0.1 [55.8 kB]
Get:46 http://archive.ubuntu.com/ubuntu xenial/main amd64 tcpd amd64 7.6.q-25 [23.0 kB]
Get:47 http://archive.ubuntu.com/ubuntu xenial/main amd64 ssh-import-id all 5.5-0ubuntu1 [10.2 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 10.5 MB in 7s (1441 kB/s)
Selecting previously unselected package libssl1.0.0:amd64.
(Reading database ... 4785 files and directories currently installed.)
Preparing to unpack .../libssl1.0.0_1.0.2g-1ubuntu4.20_amd64.deb ...
Unpacking libssl1.0.0:amd64 (1.0.2g-1ubuntu4.20) ...
Selecting previously unselected package libpython3.5-minimal:amd64.
Preparing to unpack .../libpython3.5-minimal_3.5.2-2ubuntu0~16.04.13_amd64.deb ...
Unpacking libpython3.5-minimal:amd64 (3.5.2-2ubuntu0~16.04.13) ...
Selecting previously unselected package libexpat1:amd64.
Preparing to unpack .../libexpat1_2.1.0-7ubuntu0.16.04.5_amd64.deb ...
Unpacking libexpat1:amd64 (2.1.0-7ubuntu0.16.04.5) ...
Selecting previously unselected package python3.5-minimal.
Preparing to unpack .../python3.5-minimal_3.5.2-2ubuntu0~16.04.13_amd64.deb ...
Unpacking python3.5-minimal (3.5.2-2ubuntu0~16.04.13) ...
Selecting previously unselected package python3-minimal.
Preparing to unpack .../python3-minimal_3.5.1-3_amd64.deb ...
Unpacking python3-minimal (3.5.1-3) ...
Selecting previously unselected package mime-support.
Preparing to unpack .../mime-support_3.59ubuntu1_all.deb ...
Unpacking mime-support (3.59ubuntu1) ...
Selecting previously unselected package libmpdec2:amd64.
Preparing to unpack .../libmpdec2_2.4.2-1_amd64.deb ...
Unpacking libmpdec2:amd64 (2.4.2-1) ...
Selecting previously unselected package libsqlite3-0:amd64.
Preparing to unpack .../libsqlite3-0_3.11.0-1ubuntu1.5_amd64.deb ...
Unpacking libsqlite3-0:amd64 (3.11.0-1ubuntu1.5) ...
Selecting previously unselected package libpython3.5-stdlib:amd64.
Preparing to unpack .../libpython3.5-stdlib_3.5.2-2ubuntu0~16.04.13_amd64.deb ...
Unpacking libpython3.5-stdlib:amd64 (3.5.2-2ubuntu0~16.04.13) ...
Selecting previously unselected package python3.5.
Preparing to unpack .../python3.5_3.5.2-2ubuntu0~16.04.13_amd64.deb ...
Unpacking python3.5 (3.5.2-2ubuntu0~16.04.13) ...
Selecting previously unselected package libpython3-stdlib:amd64.
Preparing to unpack .../libpython3-stdlib_3.5.1-3_amd64.deb ...
Unpacking libpython3-stdlib:amd64 (3.5.1-3) ...
Selecting previously unselected package dh-python.
Preparing to unpack .../dh-python_2.20151103ubuntu1.2_all.deb ...
Unpacking dh-python (2.20151103ubuntu1.2) ...
Processing triggers for libc-bin (2.23-0ubuntu11.3) ...
Setting up libssl1.0.0:amd64 (1.0.2g-1ubuntu4.20) ...
debconf: unable to initialize frontend: Dialog
debconf: (TERM is not set, so the dialog frontend is not usable.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.22.1 /usr/local/share/perl/5.22.1 /usr/lib/x86_64-linux-gnu/perl5/5.22 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.22 /usr/share/perl/5.22 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base .) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up libpython3.5-minimal:amd64 (3.5.2-2ubuntu0~16.04.13) ...
Setting up libexpat1:amd64 (2.1.0-7ubuntu0.16.04.5) ...
Setting up python3.5-minimal (3.5.2-2ubuntu0~16.04.13) ...
Setting up python3-minimal (3.5.1-3) ...
Processing triggers for libc-bin (2.23-0ubuntu11.3) ...
Selecting previously unselected package python3.
(Reading database ... 5761 files and directories currently installed.)
Preparing to unpack .../python3_3.5.1-3_amd64.deb ...
Unpacking python3 (3.5.1-3) ...
Selecting previously unselected package libxau6:amd64.
Preparing to unpack .../libxau6_1%3a1.0.8-1_amd64.deb ...
Unpacking libxau6:amd64 (1:1.0.8-1) ...
Selecting previously unselected package libxdmcp6:amd64.
Preparing to unpack .../libxdmcp6_1%3a1.1.2-1.1_amd64.deb ...
Unpacking libxdmcp6:amd64 (1:1.1.2-1.1) ...
Selecting previously unselected package libxcb1:amd64.
Preparing to unpack .../libxcb1_1.11.1-1ubuntu1_amd64.deb ...
Unpacking libxcb1:amd64 (1.11.1-1ubuntu1) ...
Selecting previously unselected package libx11-data.
Preparing to unpack .../libx11-data_2%3a1.6.3-1ubuntu2.2_all.deb ...
Unpacking libx11-data (2:1.6.3-1ubuntu2.2) ...
Selecting previously unselected package libx11-6:amd64.
Preparing to unpack .../libx11-6_2%3a1.6.3-1ubuntu2.2_amd64.deb ...
Unpacking libx11-6:amd64 (2:1.6.3-1ubuntu2.2) ...
Selecting previously unselected package libxext6:amd64.
Preparing to unpack .../libxext6_2%3a1.3.3-1_amd64.deb ...
Unpacking libxext6:amd64 (2:1.3.3-1) ...
Selecting previously unselected package libwrap0:amd64.
Preparing to unpack .../libwrap0_7.6.q-25_amd64.deb ...
Unpacking libwrap0:amd64 (7.6.q-25) ...
Selecting previously unselected package libmagic1:amd64.
Preparing to unpack .../libmagic1_1%3a5.25-2ubuntu1.4_amd64.deb ...
Unpacking libmagic1:amd64 (1:5.25-2ubuntu1.4) ...
Selecting previously unselected package file.
Preparing to unpack .../file_1%3a5.25-2ubuntu1.4_amd64.deb ...
Unpacking file (1:5.25-2ubuntu1.4) ...
Selecting previously unselected package libbsd0:amd64.
Preparing to unpack .../libbsd0_0.8.2-1ubuntu0.1_amd64.deb ...
Unpacking libbsd0:amd64 (0.8.2-1ubuntu0.1) ...
Selecting previously unselected package libidn11:amd64.
Preparing to unpack .../libidn11_1.32-3ubuntu1.2_amd64.deb ...
Unpacking libidn11:amd64 (1.32-3ubuntu1.2) ...
Selecting previously unselected package openssl.
Preparing to unpack .../openssl_1.0.2g-1ubuntu4.20_amd64.deb ...
Unpacking openssl (1.0.2g-1ubuntu4.20) ...
Selecting previously unselected package ca-certificates.
Preparing to unpack .../ca-certificates_20210119~16.04.1_all.deb ...
Unpacking ca-certificates (20210119~16.04.1) ...
Selecting previously unselected package krb5-locales.
Preparing to unpack .../krb5-locales_1.13.2+dfsg-5ubuntu2.2_all.deb ...
Unpacking krb5-locales (1.13.2+dfsg-5ubuntu2.2) ...
Selecting previously unselected package libedit2:amd64.
Preparing to unpack .../libedit2_3.1-20150325-1ubuntu2_amd64.deb ...
Unpacking libedit2:amd64 (3.1-20150325-1ubuntu2) ...
Selecting previously unselected package libkrb5support0:amd64.
Preparing to unpack .../libkrb5support0_1.13.2+dfsg-5ubuntu2.2_amd64.deb ...
Unpacking libkrb5support0:amd64 (1.13.2+dfsg-5ubuntu2.2) ...
Selecting previously unselected package libk5crypto3:amd64.
Preparing to unpack .../libk5crypto3_1.13.2+dfsg-5ubuntu2.2_amd64.deb ...
Unpacking libk5crypto3:amd64 (1.13.2+dfsg-5ubuntu2.2) ...
Selecting previously unselected package libkeyutils1:amd64.
Preparing to unpack .../libkeyutils1_1.5.9-8ubuntu1_amd64.deb ...
Unpacking libkeyutils1:amd64 (1.5.9-8ubuntu1) ...
Selecting previously unselected package libkrb5-3:amd64.
Preparing to unpack .../libkrb5-3_1.13.2+dfsg-5ubuntu2.2_amd64.deb ...
Unpacking libkrb5-3:amd64 (1.13.2+dfsg-5ubuntu2.2) ...
Selecting previously unselected package libgssapi-krb5-2:amd64.
Preparing to unpack .../libgssapi-krb5-2_1.13.2+dfsg-5ubuntu2.2_amd64.deb ...
Unpacking libgssapi-krb5-2:amd64 (1.13.2+dfsg-5ubuntu2.2) ...
Selecting previously unselected package libxmuu1:amd64.
Preparing to unpack .../libxmuu1_2%3a1.1.2-2_amd64.deb ...
Unpacking libxmuu1:amd64 (2:1.1.2-2) ...
Selecting previously unselected package openssh-client.
Preparing to unpack .../openssh-client_1%3a7.2p2-4ubuntu2.10_amd64.deb ...
Unpacking openssh-client (1:7.2p2-4ubuntu2.10) ...
Selecting previously unselected package wget.
Preparing to unpack .../wget_1.17.1-1ubuntu1.5_amd64.deb ...
Unpacking wget (1.17.1-1ubuntu1.5) ...
Selecting previously unselected package xauth.
Preparing to unpack .../xauth_1%3a1.0.9-1ubuntu2_amd64.deb ...
Unpacking xauth (1:1.0.9-1ubuntu2) ...
Selecting previously unselected package ncurses-term.
Preparing to unpack .../ncurses-term_6.0+20160213-1ubuntu1_all.deb ...
Unpacking ncurses-term (6.0+20160213-1ubuntu1) ...
Selecting previously unselected package openssh-sftp-server.
Preparing to unpack .../openssh-sftp-server_1%3a7.2p2-4ubuntu2.10_amd64.deb ...
Unpacking openssh-sftp-server (1:7.2p2-4ubuntu2.10) ...
Selecting previously unselected package openssh-server.
Preparing to unpack .../openssh-server_1%3a7.2p2-4ubuntu2.10_amd64.deb ...
Unpacking openssh-server (1:7.2p2-4ubuntu2.10) ...
Selecting previously unselected package python3-pkg-resources.
Preparing to unpack .../python3-pkg-resources_20.7.0-1_all.deb ...
Unpacking python3-pkg-resources (20.7.0-1) ...
Selecting previously unselected package python3-chardet.
Preparing to unpack .../python3-chardet_2.3.0-2_all.deb ...
Unpacking python3-chardet (2.3.0-2) ...
Selecting previously unselected package python3-six.
Preparing to unpack .../python3-six_1.10.0-3_all.deb ...
Unpacking python3-six (1.10.0-3) ...
Selecting previously unselected package python3-urllib3.
Preparing to unpack .../python3-urllib3_1.13.1-2ubuntu0.16.04.4_all.deb ...
Unpacking python3-urllib3 (1.13.1-2ubuntu0.16.04.4) ...
Selecting previously unselected package python3-requests.
Preparing to unpack .../python3-requests_2.9.1-3ubuntu0.1_all.deb ...
Unpacking python3-requests (2.9.1-3ubuntu0.1) ...
Selecting previously unselected package tcpd.
Preparing to unpack .../tcpd_7.6.q-25_amd64.deb ...
Unpacking tcpd (7.6.q-25) ...
Selecting previously unselected package ssh-import-id.
Preparing to unpack .../ssh-import-id_5.5-0ubuntu1_all.deb ...
Unpacking ssh-import-id (5.5-0ubuntu1) ...
Processing triggers for libc-bin (2.23-0ubuntu11.3) ...
Processing triggers for systemd (229-4ubuntu21.31) ...
Setting up mime-support (3.59ubuntu1) ...
Setting up libmpdec2:amd64 (2.4.2-1) ...
Setting up libsqlite3-0:amd64 (3.11.0-1ubuntu1.5) ...
Setting up libpython3.5-stdlib:amd64 (3.5.2-2ubuntu0~16.04.13) ...
Setting up python3.5 (3.5.2-2ubuntu0~16.04.13) ...
Setting up libpython3-stdlib:amd64 (3.5.1-3) ...
Setting up libxau6:amd64 (1:1.0.8-1) ...
Setting up libxdmcp6:amd64 (1:1.1.2-1.1) ...
Setting up libxcb1:amd64 (1.11.1-1ubuntu1) ...
Setting up libx11-data (2:1.6.3-1ubuntu2.2) ...
Setting up libx11-6:amd64 (2:1.6.3-1ubuntu2.2) ...
Setting up libxext6:amd64 (2:1.3.3-1) ...
Setting up libwrap0:amd64 (7.6.q-25) ...
Setting up libmagic1:amd64 (1:5.25-2ubuntu1.4) ...
Setting up file (1:5.25-2ubuntu1.4) ...
Setting up libbsd0:amd64 (0.8.2-1ubuntu0.1) ...
Setting up libidn11:amd64 (1.32-3ubuntu1.2) ...
Setting up openssl (1.0.2g-1ubuntu4.20) ...
Setting up ca-certificates (20210119~16.04.1) ...
debconf: unable to initialize frontend: Dialog
debconf: (TERM is not set, so the dialog frontend is not usable.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.22.1 /usr/local/share/perl/5.22.1 /usr/lib/x86_64-linux-gnu/perl5/5.22 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.22 /usr/share/perl/5.22 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base .) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up krb5-locales (1.13.2+dfsg-5ubuntu2.2) ...
Setting up libedit2:amd64 (3.1-20150325-1ubuntu2) ...
Setting up libkrb5support0:amd64 (1.13.2+dfsg-5ubuntu2.2) ...
Setting up libk5crypto3:amd64 (1.13.2+dfsg-5ubuntu2.2) ...
Setting up libkeyutils1:amd64 (1.5.9-8ubuntu1) ...
Setting up libkrb5-3:amd64 (1.13.2+dfsg-5ubuntu2.2) ...
Setting up libgssapi-krb5-2:amd64 (1.13.2+dfsg-5ubuntu2.2) ...
Setting up libxmuu1:amd64 (2:1.1.2-2) ...
Setting up openssh-client (1:7.2p2-4ubuntu2.10) ...
Setting up wget (1.17.1-1ubuntu1.5) ...
Setting up xauth (1:1.0.9-1ubuntu2) ...
Setting up ncurses-term (6.0+20160213-1ubuntu1) ...
Setting up openssh-sftp-server (1:7.2p2-4ubuntu2.10) ...
Setting up openssh-server (1:7.2p2-4ubuntu2.10) ...
debconf: unable to initialize frontend: Dialog
debconf: (TERM is not set, so the dialog frontend is not usable.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.22.1 /usr/local/share/perl/5.22.1 /usr/lib/x86_64-linux-gnu/perl5/5.22 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.22 /usr/share/perl/5.22 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base .) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Creating SSH2 RSA key; this may take some time ...
2048 SHA256:rAFUAwYa7eGhBfIMy2JzxT9UAdRQNg3Fpx0r4jMvmKc root@91f90eae75ef (RSA)
Creating SSH2 DSA key; this may take some time ...
1024 SHA256:XJBKiRYkMKndipPo6gXoarB/KsJ+hBXCrXq9ZlkQ2A4 root@91f90eae75ef (DSA)
Creating SSH2 ECDSA key; this may take some time ...
256 SHA256:fgeVtLB9MOVhSf+4ycGSl6I5yW5fEMWwoVZPzyLhkt8 root@91f90eae75ef (ECDSA)
Creating SSH2 ED25519 key; this may take some time ...
256 SHA256:kQE83fLfw0nAku7Jc09csIjjk0MQ7i1uJSWaYPyl/DI root@91f90eae75ef (ED25519)
invoke-rc.d: could not determine current runlevel
invoke-rc.d: policy-rc.d denied execution of start.
Setting up tcpd (7.6.q-25) ...
Setting up dh-python (2.20151103ubuntu1.2) ...
Setting up python3 (3.5.1-3) ...
running python rtupdate hooks for python3.5...
running python post-rtupdate hooks for python3.5...
Setting up python3-pkg-resources (20.7.0-1) ...
Setting up python3-chardet (2.3.0-2) ...
Setting up python3-six (1.10.0-3) ...
Setting up python3-urllib3 (1.13.1-2ubuntu0.16.04.4) ...
Setting up python3-requests (2.9.1-3ubuntu0.1) ...
Setting up ssh-import-id (5.5-0ubuntu1) ...
Processing triggers for libc-bin (2.23-0ubuntu11.3) ...
Processing triggers for ca-certificates (20210119~16.04.1) ...
Updating certificates in /etc/ssl/certs...
129 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
Processing triggers for systemd (229-4ubuntu21.31) ...
Removing intermediate container 91f90eae75ef
 ---> 9a139ac4725f
Step 4/12 : RUN mkdir -p /var/run/sshd
 ---> Running in aa00047a0552
Removing intermediate container aa00047a0552
 ---> 49de55763402
Step 5/12 : RUN echo 'root:root' | chpasswd
 ---> Running in 0e5905896793
Removing intermediate container 0e5905896793
 ---> 41e96b58643a
Step 6/12 : RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
 ---> Running in 4494c577094d
Removing intermediate container 4494c577094d
 ---> e284d0b88735
Step 7/12 : RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
 ---> Running in d1dedf68b655
Removing intermediate container d1dedf68b655
 ---> c244a4f3376c
Step 8/12 : RUN mkdir -p /root/.ssh
 ---> Running in 4ff66790bdeb
Removing intermediate container 4ff66790bdeb
 ---> 659111e1bb0c
Step 9/12 : COPY authorized_keys /root/.ssh/authorized_keys
 ---> df510e2edaf0
Step 10/12 : EXPOSE 22
 ---> Running in f24c75648a96
Removing intermediate container f24c75648a96
 ---> fe9a768161b4
Step 11/12 : EXPOSE 80
 ---> Running in 8b1dc3c55e6f
Removing intermediate container 8b1dc3c55e6f
 ---> 81e616216970
Step 12/12 : CMD ["/usr/sbin/sshd", "-D"]
 ---> Running in 6b264b757de7
Removing intermediate container 6b264b757de7
 ---> 4765e1707c51
Successfully built 4765e1707c51
Successfully tagged tektutor/ansible-ubuntu-node:latest  
</pre>

## Lab - Creating ubuntu1 and ubuntu2 ansible node containers from our custom docker image
```
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ docker images     
REPOSITORY                                   TAG            IMAGE ID       CREATED         SIZE
tektutor/ansible-ubuntu-node                 latest         4765e1707c51   4 minutes ago   220MB
tektutor/hello-ms-build                      latest         fd7be7f2409e   28 hours ago    408MB
<none>                                       <none>         929e75d34295   28 hours ago    761MB
nginx                                        latest         eea7b3dcba7e   9 days ago      187MB
ubuntu                                       22.04          01f29b872827   3 weeks ago     77.8MB
registry.access.redhat.com/ubi8/openjdk-11   latest         998e41ed2c35   5 weeks ago     391MB
ubuntu                                       16.04          b6f507652425   24 months ago   135MB
maven                                        3.6.3-jdk-11   e23b595c92ad   2 years ago     658MB
google/pause                                 latest         f9d5de079539   9 years ago     240kB
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ docker run -d --name ubuntu1 --hostname ubuntu1 -p 2001:22 -p 8001:80 tektutor/ansible-ubuntu-node:latest 
c901959cc158aa2f8007f8d48b44a55973656d0fe3eb060ae751cbc912e50ced
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ docker run -d --name ubuntu2 --hostname ubuntu2 -p 2002:22 -p 8002:80 tektutor/ansible-ubuntu-node:latest
a6c28fa2693acf56c15dff06523cc84598da5a7203542bbc5b8559e13d8a5a31
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ docker ps                                                                                                
CONTAINER ID   IMAGE                                 COMMAND               CREATED          STATUS          PORTS                                                                          NAMES
a6c28fa2693a   tektutor/ansible-ubuntu-node:latest   "/usr/sbin/sshd -D"   7 seconds ago    Up 7 seconds    0.0.0.0:2002->22/tcp, :::2002->22/tcp, 0.0.0.0:8002->80/tcp, :::8002->80/tcp   ubuntu2
c901959cc158   tektutor/ansible-ubuntu-node:latest   "/usr/sbin/sshd -D"   21 seconds ago   Up 20 seconds   0.0.0.0:2001->22/tcp, :::2001->22/tcp, 0.0.0.0:8001->80/tcp, :::8001->80/tcp   ubuntu1
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ ssh -p 2001 root@localhost
The authenticity of host '[localhost]:2001 ([::1]:2001)' can't be established.
ED25519 key fingerprint is SHA256:kQE83fLfw0nAku7Jc09csIjjk0MQ7i1uJSWaYPyl/DI.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[localhost]:2001' (ED25519) to the list of known hosts.
Welcome to Ubuntu 16.04.7 LTS (GNU/Linux 6.3.0-kali1-amd64 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@ubuntu1:~# exit
logout
Connection to localhost closed.
                                                                                                                
┌──(jegan㉿tektutor.org)-[~/openshift-aug-2023/Day5/ubuntu-ansible]
└─$ ssh -p 2002 root@localhost
The authenticity of host '[localhost]:2002 ([::1]:2002)' can't be established.
ED25519 key fingerprint is SHA256:kQE83fLfw0nAku7Jc09csIjjk0MQ7i1uJSWaYPyl/DI.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:1: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[localhost]:2002' (ED25519) to the list of known hosts.
Welcome to Ubuntu 16.04.7 LTS (GNU/Linux 6.3.0-kali1-amd64 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@ubuntu2:~# exit
logout
Connection to localhost closed.
</pre>
