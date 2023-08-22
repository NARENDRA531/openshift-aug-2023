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

## Docker SWARM Overview

## Kubernetes Overview

## Red Hat OpenShift Overview


