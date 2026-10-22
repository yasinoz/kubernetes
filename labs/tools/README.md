---
title: Tools
lang: en
slug: tools
section: production
order: 3
duration: 40
requires: [kubernetes, helm]
status: updated
upstream: labs/tools
---
# Tools

Kubectl is great but sometimes you need something else to help you navigate your cluster.

These exercises will walk you through some of the more popular tools.

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

Takes about 40 minutes.

A **Kubernetes cluster** must be running. Docker Desktop ships one:
*Settings -> Kubernetes -> Enable Kubernetes*. Check it:

```
kubectl get nodes
```

You should see at least one node with `STATUS` of `Ready`. If the command
hangs or errors, the cluster is still starting - give it a minute and try
again.

**Helm** must be installed - it is a separate command line tool, it
does not come with Docker Desktop. Check it:

```
helm version --short
```

A version number like `v3.x.x` means you are ready. If you get
`command not found`, install it first: on Windows `winget install Helm.Helm`,
on macOS `brew install helm`, or follow
[helm.sh/docs/intro/install](https://helm.sh/docs/intro/install/).

> Run every command from the **root of this repository** - the paths in this lab are written relative to it.

## Reference

- [Dashboard - web UI](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)
- [K9s - console UI](https://github.com/derailed/k9s)
- [Kubectl plugins - Krew](https://krew.sigs.k8s.io/plugins/)
- [Kubewatch - chat notification](https://github.com/bitnami-labs/kubewatch)
___

## * **Do this first if you use Docker Desktop** *

There's a [bug in the default RBAC setup](https://github.com/docker/for-mac/issues/4774) in Docker Desktop, which means permissions are not applied correctly. If you're using Kubernetes in Docker Desktop, run this to fix the bug:

```
# on Docker Desktop for Mac (or WSL2 on Windows):
sudo chmod +x ./scripts/fix-rbac-docker-desktop.sh
./scripts/fix-rbac-docker-desktop.sh

# OR on Docker Desktop for Windows (PowerShell):
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope Process -Force
./scripts/fix-rbac-docker-desktop.ps1
```
___

## Dashboard

This is the standard web UI for the Kubernetes project:

![](../../img/tools-dashboard-pods.png)

Deploy metrics server:

```
kubectl top nodes

# if you get no response, then install the components:

kubectl apply -f labs/tools/specs/metrics-server
```

Deploy some resources so we have something to see:

```
kubectl apply -f labs/tools/specs/rng
```

> Check the app is working at http://localhost:30080 - click Go and you should see a random number


Now deploy the dasboard:

```
kubectl apply -f labs/tools/specs/dashboard
```

This setup includes a ServiceAccount we can use to authenticate with the web UI:

```
kubectl describe sa rng-admin-user -n kubernetes-dashboard

kubectl get secret -n kubernetes-dashboard rng-admin-user-token
```

> SA auth tokens are stored in Secrets of the special type `kubernetes.io/service-account-token`. Wnen you create those Secrets, Kubernetes generates the token

Print the auth token:

```
kubectl -n kubernetes-dashboard get secret rng-admin-user-token -o go-template="{{.data.token | base64decode}}"
```

> Copy the value to the clipboard, and use it to log in at https://localhost:30043

- Accept security risk - this deployment uses a self-signed SSL cert
- Paste token - those are your admin creds
- Empty screens, browse around - change ns
- Can view Pods etc. in all ns - not secrets
- Pods - check logs, exec 
- Can view all in rng ns
- Can edit only in rng ns

This SA doesn't have full permissions, so you can only edit in the `rng` namespace.

![](../../img/tools-dashboard-edit.png)

## K9s

K9s is terminal-based GUI for navigating Kubernetes clusters: https://github.com/derailed/k9s#installation

![](../../img/tools-k9s-pods.png)


```
# Windows - you need to be an admin user
choco install k9s

# Mac
brew install k9s

# Linux
curl -sS https://webinstall.dev/k9s | bash
```

Run in read-only mode - this uses your default cluster admin context:

```
k9s --readonly
```

Keyboard navigation 

- numbers to switch ns
- 0 for all
- up/down to select pod
- l for logs
- esc to go back

Switch resources:

- :svc
- :cm
- :secrets - x to decode


![](../../img/tools-k9s-secret.png)

Ctrl-C to exit

Create a context for the RNG admin user

```
kubectl -n kubernetes-dashboard get secret rng-admin-user-token -o go-template="{{.data.token | base64decode}}"

kubectl config set-credentials rng-admin --token=<your-sa-token>

kubectl config get-contexts  

kubectl config set-context rng --user=rng-admin --cluster=<your-cluster-name>
```

Launch k9s as rng user:

```
k9s -n rng --context rng
```

- navigate, can see pods but only shell into rng pods
- :secrets - error
- back to :pods, select rng ns 1
- :secrets - u to check usage


## Kubectl plugins - Krew

You can add plugins to extend Kubectl's functionality - Krew is a plugin manager.

Start by installing Krew - the setup is different for different OS: https://krew.sigs.k8s.io/docs/user-guide/setup/install/

> Add to path and restart shell (or restart Code)

```
kubectl krew 

kubectl krew search rbac
```

> Will likely need admin permissions 

```
kubectl krew install rbac-view

kubectl rbac-view
```

> Browse to URL; comprehensive but unwieldy

```
kubectl krew install who-can

kubectl who-can get secrets -n rng
```

> Not complete

```
kubectl auth can-i get secrets -n rng --as system:serviceaccount:kubernetes-dashboard:rng-admin-user
```

```
kubectl krew install access-matrix

kubectl access-matrix --sa kubernetes-dashboard:rng-admin-user

kubectl access-matrix --sa kubernetes-dashboard:rng-admin-user -n rng
```

## Kubewatch (Slack integration)

This is a great tool for notifying you about changes in your environment. You need admin access to a Slack workspace to set it up.

![](../../img/tools-kubewatch-slack.png)

Slack - create new workspace, call the first channel lab-tools

- https://slack.com/intl/en-gb/get-started#/createnew

Add the bot app to get an API token, call it kubewatch:

- https://courselabsworkspace.slack.com/apps/A0F7YS25R-bots?utm_source=in-prod&utm_medium=inprod-btn_app_install-index-click&tab=more_info

- in the channel - `/invite @kubewatch`

Install the server componentes with Helm:

```
helm repo add bitnami https://charts.bitnami.com/bitnami

helm install kubewatch -n default --values=labs/tools/kubewatch/values.yaml --set='slack.channel=#lab-tools,slack.token=<YOUR-TOKEN>' bitnami/kubewatch
```

Check the install:

```
kubectl logs -n default -l app.kubernetes.io/name=kubewatch
```

> If you see an RBAC issue, it can be ignored

Try deleting a Pod and see the notifications in Slack:

```
kubectl delete po -n rng -l app=numbers-api
```


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

Delete the namespaces & RBAC used for the RNG app and the Dashboard:

```
kubectl delete ns,clusterrolebinding -l kubernetes.courselabs.co=tools
```

Delete metrics server if you deployed it:

```
kubectl delete -f labs/tools/specs/metrics-server
```

And uninstall Kubewatch if you deployed it:

```
helm uninstall kubewatch
```
