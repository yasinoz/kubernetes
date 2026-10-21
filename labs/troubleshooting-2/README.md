---
title: Troubleshooting 2
lang: en
slug: troubleshooting-2
section: real-world
order: 2
duration: 60
requires: [kubernetes]
status: needs-review
upstream: labs/troubleshooting-2
---
# Troubleshooting Application Modeling in Kubernetes

Modelling apps in Kubernetes involves multiple resources, Even simple apps use ConfigMaps, Secrets, PersistentVolumes, and Namespaces. Any mistakes in configuration will make apps fail in interesting ways.

<!-- prereqs:auto -->
## Before you start

Takes about 60 minutes.

A **Kubernetes cluster** must be running. Docker Desktop ships one:
*Settings -> Kubernetes -> Enable Kubernetes*. Check it:

```
kubectl get nodes
```

You should see at least one node with `STATUS` of `Ready`. If the command
hangs or errors, the cluster is still starting - give it a minute and try
again.

> Run every command from the **root of this repository** - the paths in this lab are written relative to it.

## Lab

This lab presents a (pretend) web application that reads configuration from a ConfigMap, database credentials from a Secret, and stores data in a PersistentVolume. The app is deployed to a custom namespace.

Try running this app - and make whatever changes you need to get the app running, so the Pod is healthy and you can access the application.

```
kubectl apply -f labs/troubleshooting-2/specs/
```

> Your goal is to browse to http://localhost:8040 or http://localhost:30040 and see the web app displaying its configuration

Don't go straight to the solution! These configuration issues are common in real deployments, so it's important to practice diagnosing and fixing them.

> Stuck? Try [hints](hints.md) or check the [solution](solution.md).

___
<!-- troubleshooting:auto -->
## If something goes wrong

**`The connection to the server ... was refused`** - the cluster is not
running. Enable Kubernetes in Docker Desktop and wait for it to start.

**`provided port is already allocated`** - a Service from an earlier lab still
holds that NodePort. Find it with `kubectl get svc -A` and delete the namespace
that lab created.

**`failed calling webhook ... service not found`** - leftovers from the
**admission** lab are blocking the API server, and you will not be able to
create anything, not even a namespace. That lab's Cleanup section has the two
commands that remove the webhook configurations.

**Pod stuck in `ContainerCreating` or `Pending`** - usually the image is still
downloading, which can take a few minutes the first time. Check with
`kubectl describe pod <name>` and read the **Events** at the bottom: they say
exactly what the Pod is waiting for.

**Pod in `CrashLoopBackOff`** - the container starts and exits. The reason is
in its logs: `kubectl logs <name>`. Add `--previous` to see the run before the
last restart.

Still stuck? The **Cleanup** section below resets everything, so you can start
the lab again from a clean cluster.


## Cleanup

When you're done you can remove all the objects:

```
kubectl delete ns troubleshooting-2

kubectl delete cm -A -l kubernetes.courselabs.co=troubleshooting-2
```
