# Hardening Kubernetes from Scratch

The community continues to benefit from  ```kubernetes-the-hard-way``` by Kelsey Hightower in understanding how each of the components work together and are configured in a secure manner, step-by-step.  In a similar manner but using a slightly different approach, this guide seeks to demonstrate how the security-related settings inside ```kubernetes``` and ```etcd``` actually work from the ground up, one change at a time, validated by real attacks.

By following this guide, you will configure one of the least secure clusters possible at the start. Each step will attempt to follow the pattern of a) educate, b) attack, c) harden, and d) verify in order of security importance and maturity.  Upon completion of the guide, you will have successfully hacked your cluster several times over and now fully understand all the necessary configuration changes to prevent each one from ever happening again.

> The cluster built in this tutorial is not production ready--especially at the beginning--but the concepts learned are definitely applicable to your production clusters.

## Target Audience

The target audience for this tutorial is someone who has a working knowledge of running a Kubernetes cluster (or has completed the ```kubernetes-the-hard-way``` tutorial) and wants to understand how each security-related setting works at a deep level.

## Versions Targeted

- Ubuntu 16.0.4 LTS
- Docker 1.13.x
- Kubernetes 1.9.2

## Pre-Requisite Tools

- bash
- git
- AWS cli tools
- Valid AWS Account Credentials with full ec2 permissons
- dig
- kubectl 

## The (Purposefully) Insecure Cluster

### Cluster Details

- Etcd
- Master
- Worker1
- Worker2

### Diagram/Structure

TODO

## Basic Infrastructure Installation

- Create the VPC
- Launch and configure the ```etcd``` instance
- Launch and configure the ```master``` instance
- Launch and configure the ```worker-1``` and ```worker-2``` instance

## Deploy Admin Workloads

- Heapster
- Dashboard

## Deploy Application Workloads
- Nginx
- Vulnapp
- Azure Vote App

## Event #1 - Pentest - Direct, External Network Attacks

### Etcd

curl etcd endpoint - SG

### API

curl API endpoint - SG

### Kubelet

curl Kubelet API - SG

### NodePorts

Port scan NodePorts - SG

### Metrics

Port scan - SG

### Administrative SSH

Admin - SG

## Event #2  - Pentest - Kubernetes API Attacks

### API Authentication

#### No Auth

TODO

#### Basic Auth

TODO

#### TLS Authentication

TODO

## Event #3 - Another Admin Joins the Team

### Shared Admin Credentials

TODO

### Separate Namespaces

TODO

### Namespace Resource Quota

TODO

## Event #4 - A Contract Developer Joins the Cluster

### Sample Vuln Application - Image Sourcing

TODO

## Event #5 - Admin Leaves the Team

###  Shared Admin Credentials

TODO

## Event #6 - In Production - Vote is Hacked

### Investigation - Backdoored App

TODO

#### Open Etcd -> TLS

TODO

#### Kubernetes Insecure Port -> Remove

TODO

#### Admission Controllers -> Enable

TODO

#### RBAC -> Enable

TODO

#### Kubelet Authn/z -> Enable

TODO

#### Open Dashboard -> Upgrade

TODO

#### Pods can talk everywhere -> Enable Network Policy

TODO

#### Approved Image Sources

TODO

## Event #7 - New Admin Joins

### Telemetry

#### Metrics

TODO

#### Logging

TODO

### Helm/Tiller

TODO

### Istio Service Mesh

TODO

## Event #8 - Final Pentest - Advanced Hardening

### Image Scanning

TODO

### PodSecurityPolicy

TODO

### Behavior Scanning

TODO

## Clean Up

- Delete Instances
- Delete Old Keys and Configs
- Delete VPC
