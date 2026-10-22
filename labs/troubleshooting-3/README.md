---
title: Troubleshooting 3
lang: en
slug: troubleshooting-3
section: real-world
order: 3
duration: 60
requires: [kubernetes]
status: needs-review
upstream: labs/troubleshooting-3
---
# Troubleshooting Advanced Kubernetes Components

More advanced app models use Helm for package management, along with complex resources like Ingress for HTTP routing and StatefulSets for stateful applications.

When these components are misconfigured within a Helm chart, debugging requires understanding both Helm templating and the underlying Kubernetes resources.

<!-- orientation:auto -->
## How this lab works

Read this once - every lab in this track follows the same rules.

**Where you type the commands.** Open a terminal and go to the folder where you
cloned this repository. On Windows use **PowerShell** (search for it in the
Start menu), on macOS or Linux use **Terminal**. Every path in this lab is
written from the repository root, so stay there unless a step says otherwise.

**Lines starting with `>` are explanations, not commands.** Don't paste them.
Only the text inside grey code boxes gets typed.

**The 📋 sign means it's your turn.** When you see it, try the step yourself
first. Underneath it there is a collapsed **"Not sure how?"** arrow with the
answer - open it *after* you have tried, not before. The labs are written this
way on purpose: you remember what you worked out, not what you pasted.

**If a command fails, that is normal.** Some steps fail deliberately to show
you what the error looks like. Read the sentence above the command before you
assume something is broken.

**When you get stuck**, in this order:

1. Re-read the paragraph above the command - the answer is usually there.
2. Open the **"Not sure how?"** arrow.
3. Open `hints.md` for a nudge.
4. Open `solution.md` for the full answer. Using it is fine; copying it without
   reading is not.

**When you finish**, run the **Cleanup** section at the bottom. Labs share
ports and container names, so leftovers from one lab break the next one.
<!-- /orientation:auto -->

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

This lab presents a broken application packaged as a Helm chart. The chart deploys a web frontend with Ingress routing and a PostgreSQL database using StatefulSet.

Deploy the Ingress Controller:

```
kubectl apply -f labs/ingress/specs/ingress-controller
```

Install the Helm chart:

```
helm upgrade --install broken-app labs/troubleshooting-3/specs/app-chart/ --create-namespace --namespace troubleshooting-3
```

Your goals:

1. Fix the Helm deployment so it installs successfully

2. Browse tohttp://whoami.local:8000/ and see the web app served through the Ingress

Don't go straight to the solution! Start by investigating why the Helm install fails, then work through the subsequent issues you'll discover.

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
helm uninstall broken-app -n troubleshooting-app
kubectl delete pvc --all -n troubleshooting-app
kubectl delete ns troubleshooting-app
```
