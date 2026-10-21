---
title: Troubleshooting
lang: en
slug: troubleshooting
section: real-world
order: 1
duration: 60
requires: [kubernetes]
status: needs-review
upstream: labs/troubleshooting
---
# Troubleshooting Apps in Kubernetes

You'll spend a lot of your time in Kubectl troubleshooting problems.

Kubernetes validates API specs for correctness when you deploy them, but it doesn't check that your app will actually work.

Objects like Services and Pods are loosely-coupled, so it's easy to break your application if there are errors in your specs.

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

This one is all lab :) Try running this app - and make whatever changes you need to get the app running, so the Pod is healthy with no restarts.

```
kubectl apply -f labs/troubleshooting/specs/pi-failing
```

> Your goal is to browse to http://localhost:8020 or http://localhost:30020 and see the response from the Pi app

Don't go straight to the solution! These are the sort of issues you will get all the time, so it's good to start working through the steps to diagnose problems.

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
kubectl delete all -l kubernetes.courselabs.co=troubleshooting
```
