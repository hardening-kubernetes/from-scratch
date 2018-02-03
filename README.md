# Hardening Kubernetes from Scratch

The community continues to benefit from  ```kubernetes-the-hard-way``` by Kelsey Hightower in understanding how each of the components work together and are configured in a reasonably secure manner, step-by-step.  In a similar manner but using a slightly different approach, this guide attempts to demonstrate how the security-related settings inside ```kubernetes``` actually work from the ground up, one change at a time, validated by real attacks where possible.

By following this guide, you will configure one of the least secure clusters possible at the start. Each step will attempt to follow the pattern of a) educate, b) attack, c) harden, and d) verify in order of security importance and maturity.  Upon completion of the guide, you will have successfully hacked your cluster several times over and now fully understand all the necessary configuration changes to prevent each one from happening.

> The cluster built in this tutorial is not production ready--especially at the beginning--but the concepts learned are definitely applicable to your production clusters.

## Target Audience

The target audience for this tutorial is someone who has a working knowledge of running a Kubernetes cluster (or has completed the ```kubernetes-the-hard-way``` tutorial) and wants to understand how each security-related setting works at a deep level.

## Cluster Software Details

- AWS VPC/EC2
- Ubuntu 16.0.4 LTS
- Docker 1.13.x
- CNI 0.6.0
- etcd 3.2.11
- Kubernetes 1.9.2

## Pre-Requisite Tools

- AWS Account Credentials
  - Create/delete VPC (subnets, route tables, internet gateways)
  - Create/delete Cloudformation
  - Create/delete EC2 * (security groups, keypairs, instances)
- AWS cli tools configured to use said account
- bash
- git
- dig
- kubectl (v1.9.2)

## The (Purposefully) Insecure Cluster

### Cluster System Details

- Etcd - t2.micro (10.1.0.5)
- Master - t2.small (10.1.0.10)
- Worker1 - t2.small (10.1.0.11)
- Worker2 - t2.small (10.1.0.12)

### Diagram/Structure

TODO

### Labs

#### Installing the Prerequisites
1. [AWS](docs/install-aws.md)
2. [bash](docs/install-bash.md)
3. [git](docs/install-git.md)
4. [dig](docs/install-dig.md)
5. [kubectl](docs/install-kubectl.md)
6. [cfssl](docs/install-cfssl.md)

#### Build the Cluster
1. [Create the VPC](docs/create-vpc.md)
2. [Launch and configure the ```etcd``` instance](docs/launch-configure-etcd.md)
3. [Launch and configure the ```master``` instance](docs/launch-configure-master.md)
4. [Launch and configure the ```worker-1``` and ```worker-2``` instance](docs/launch-configure-workers.md)

#### Level 0 Security
1. [Deploy kube-dns](docs/deploy-kube-dns.md)
2. [Deploy Heapster](docs/deploy-heapster.md)
3. [Deploy Dashboard](docs/deploy-basic-dashboard.md)

#### Level 0 Attacks
1. Scan Ports
2. etcdctl
3. kubectl to API
4. curl to Kubelet/metrics

#### Level 1 Hardening
1. Security Groups
2. Etcd TLS
3. Cluster TLS
4. Admission Controllers

#### Deploy Application Workloads
1. Helm/Tiller
2. Vulnapp
3. Azure Vote App

#### Level 1 Attacks
1. Multi-tenant Misuse
 - Too many Pods
 - Too many CPU/RAM shares

#### Level 2 Hardening
1. Separate Namespaces
2. Request/Limits on Pods
3. Namespace Resource Quotas
4. Metrics via Prometheus
5. Optional multi-etcd, mult-master

#### Level 2 Attacks
1. Malicious Image, Compromised Container, Multi-tenant Misuse
  - Service Account Tokens
  - Dashboard Access
  - Tiller Access
  - Kubelet Exploit
  - Application Tampering
  - Metrics Scraping
  - Metadata API
  - Outbound Scanning/pivoting 

#### Level 3 Hardening
1. RBAC
2. New Dashboard
3. Separate Kubeconfigs per user
4. Tiller TLS
5. Kubelet Authn/z
6. Network Policy/CNI
7. Admission Controllers
8. Logging?

#### Level 4 Attacks
1. Malicious Image, Compromised Container, Multi-tenant Misuse
  - Escape the container

#### Level 4 Hardening
1. Advanced admission controllers
2. Restrict images/sources
3. Network Egress filtering
4. Vuln scan images
5. Pod Security Policy
6. Encrypted etcd
7. Sysdig Falco

### Clean Up
1. [Delete Instances](docs/delete-instances.md)
2. [Delete VPC](docs/delete-vpc.md)

## Next Steps
- Kubernetes the Hard Way
