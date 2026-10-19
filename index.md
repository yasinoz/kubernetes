---
title: Kubernetes Labs
lang: en
slug: index
section: index
order: 0
status: needs-review
upstream: index.md
---
Welcome to the Kubernetes labs.

These are hands-on resources to help you learn Kubernetes.

## Pre-reqs

 - [Set up Kubernetes and a Git client](./setup/README.md) 
 - Download your repo
    - Open a terminal (PowerShell on Windows; any shell on Linux/macOS) 
    - Run: `git clone https://github.com/courselabs/kubernetes.git`
     - Open the folder: `cd kubernetes`
- _For advanced topics_
    - Install [Helm](https://helm.sh/docs/intro/install/), [ArgoCD](https://argoproj.github.io/argo-cd/getting_started/#2-download-argo-cd-cli) and [k3d](https://k3d.io/v4.4.8/#installation) command line tools
- _Optional_
    - Install [Visual Studio Code](https://code.visualstudio.com) (free - Windows, macOS and Linux) to browse the repo and documentation

<!-- labs:start -->

## Core Kubernetes

The objects you use every day.

| Lab | Time | Needs |
|---|---|---|
| [Nodes](labs/nodes/README.md) | 30 min | kubernetes |
| [Pods](labs/pods/README.md) | 45 min | kubernetes |
| [Services](labs/services/README.md) | 45 min | kubernetes |
| [Deployments](labs/deployments/README.md) | 45 min | kubernetes |

## Application Modelling

Configuration, secrets and storage.

| Lab | Time | Needs |
|---|---|---|
| [ConfigMaps](labs/configmaps/README.md) | 40 min | kubernetes |
| [Secrets](labs/secrets/README.md) | 40 min | kubernetes |
| [PersistentVolumes](labs/persistentvolumes/README.md) | 45 min | kubernetes |
| [Namespaces](labs/namespaces/README.md) | 35 min | kubernetes |

## Advanced Application Modelling

Access control and the workload types beyond Deployments.

| Lab | Time | Needs |
|---|---|---|
| [Role-based Access Control (RBAC)](labs/rbac/README.md) | 50 min | kubernetes |
| [DaemonSets](labs/daemonsets/README.md) | 35 min | kubernetes |
| [Ingress](labs/ingress/README.md) | 50 min | kubernetes |
| [Jobs and CronJobs](labs/jobs/README.md) | 40 min | kubernetes |
| [StatefulSets](labs/statefulsets/README.md) | 50 min | kubernetes |

## Operating Kubernetes

Getting a cluster ready for real traffic.

| Lab | Time | Needs |
|---|---|---|
| [Production Readiness](labs/productionizing/README.md) | 50 min | kubernetes |
| [Monitoring](labs/monitoring/README.md) | 45 min | kubernetes |
| [Logging](labs/logging/README.md) | 45 min | kubernetes |

## CI/CD

Building images and deploying them automatically.

| Lab | Time | Needs |
|---|---|---|
| [Image Optimizing](labs/docker/README.md) | 40 min | kubernetes |
| [BuildKit](labs/buildkit/README.md) | 40 min | kubernetes |
| [Helm](labs/helm/README.md) | 50 min | kubernetes |
| [Rollouts](labs/rollouts/README.md) | 45 min | kubernetes |
| [Jenkins](labs/jenkins/README.md) | 55 min | kubernetes |
| [ArgoCD](labs/argo/README.md) | 55 min | kubernetes |

## Advanced Kubernetes

Network policy, admission control and operators.

| Lab | Time | Needs |
|---|---|---|
| [NetworkPolicy](labs/networkpolicy/README.md) | 45 min | kubernetes |
| [Admission](labs/admission/README.md) | 55 min | kubernetes |
| [Operators](labs/operators/README.md) | 55 min | kubernetes |

## Production Operations

Cluster topology, scheduling and the tools you actually use.

| Lab | Time | Needs |
|---|---|---|
| [Clusters](labs/clusters/README.md) | 45 min | kubernetes |
| [Affinity](labs/affinity/README.md) | 45 min | kubernetes |
| [Tools](labs/tools/README.md) | 40 min | kubernetes |

## Real Kubernetes

Broken clusters, and how you work out why.

| Lab | Time | Needs |
|---|---|---|
| [Troubleshooting](labs/troubleshooting/README.md) | 60 min | kubernetes |
| [Troubleshooting 2](labs/troubleshooting-2/README.md) | 60 min | kubernetes |
| [Troubleshooting 3](labs/troubleshooting-3/README.md) | 60 min | kubernetes |

<!-- labs:end -->

### Credits

Created by [@EltonStoneman](https://twitter.com/EltonStoneman) ([sixeyed](https://github.com/sixeyed)): Freelance Consultant and Trainer. Author of [Learn Docker in a Month of Lunches](https://www.manning.com/books/learn-docker-in-a-month-of-lunches), [Learn Kubernetes in a Month of Lunches](https://www.manning.com/books/learn-kubernetes-in-a-month-of-lunches) and [many Pluralsight courses](https://pluralsight.pxf.io/c/1197078/424552/7490?u=https%3A%2F%2Fwww.pluralsight.com%2Fauthors%2Felton-stoneman).
