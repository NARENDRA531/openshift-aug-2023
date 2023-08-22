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

## Docker SWARM Overview

## Kubernetes Overview

## Red Hat OpenShift Overview


