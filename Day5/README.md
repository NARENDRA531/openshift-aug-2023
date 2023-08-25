# Day 5

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

cd Day5/helm/wordpress
helm package wordpress
helm install wp wordpress-0.1.0.tgz
helm list
```
