---
title: Nodes
lang: en
slug: nodes
section: core
order: 1
duration: 30
requires: [kubernetes]
status: updated
upstream: labs/nodes
---
# Examining Nodes with Kubectl

Nodes are the machines in your cluster which actually run containers. Kubernetes stores information about nodes in its database and you can query it using the command line.

[Kubectl](https://kubectl.docs.kubernetes.io/references/kubectl/) is the command line to work with a Kubernetes cluster. It has commands to deploy applications and work with objects. 

This is a simple lab to get you used to working with Kubectl, and to see some of the interesting things Kubernetes knows about the machines in the cluster.

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

Takes about 30 minutes.

A **Kubernetes cluster** must be running. Docker Desktop ships one:
*Settings -> Kubernetes -> Enable Kubernetes*. Check it:

```
kubectl get nodes
```

You should see at least one node with `STATUS` of `Ready`. If the command
hangs or errors, the cluster is still starting - give it a minute and try
again.

> Run every command from the **root of this repository** - the paths in this lab are written relative to it.

## Working with Nodes

Two of the most common Kubectl commands are `get` and `describe`.

You can use them with different objects, by adding the object type to the command:

```
kubectl get nodes
```

> This shows all the servers in the cluster.

The `get` command prints a table with basic information. You use it to find out what objects exist. 

To see the next level of detail for an object, use the `describe` command:

``` 
kubectl describe nodes
```

> There's a lot more information to see and `describe` gives it to you in a readable format.

If you have several nodes in your cluster (which you will do for all production Kubernetes deployments), you can print the details for one node by adding its name to the `decribe` command:

```
# replace <node-name> with the name of one of your nodes:
kubectl describe node <node-name>

# e.g. on Docker Desktop:
# kubectl describe node docker-desktop
```

📋 Can you use the output to see how hard your node is working?

<details>
  <summary>Not sure how?</summary>

Kubernetes knows the compute capacity of each node - how many CPU cores and how much memory it has. That's shown in the output, along with the workloads currently running on the node and the amount of CPU and memory which has been allocated.

</details><br/>

## Working with Kubectl

Kubectl has built-in help, you can use it to list all commands or list the details of one command:

```
kubectl --help

kubectl get --help
```

And you can learn about resources by asking Kubectl to explain them:

```
kubectl explain node
```

> This doesn't tell you much, but it gives you a brief description of the resource. Nodes are just like any other resource in Kubernetes, their status is monitored and recorded using a fixed data schema.

## Querying and formatting

You will spend **a lot** of time with Kubectl. You'll want to get familiar with some features early on, like querying.

Kubectl can print information in different formats, try showing your node details in JSON:

```
kubectl get node <node-name> -o json

# e.g. on Docker Desktop
# kubectl get node docker-desktop -o json
```

📋 Check the help to see what other output formats you can use.

<details>
  <summary>Not sure how?</summary>

`kubectl --help` gives you the top-level information, showing you the commands you can run. 

To get the options for the `get` command run:

```
kubectl get --help
```

That shows the output formats available with `-o` (or `--output`), something like this:

```
-o, --output='': Output format. One of:
json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-as-json|jsonpath-file|custom-columns-file|custom-columns|wide
```

</details><br/>

One output option is [JSON Path](https://kubernetes.io/docs/reference/kubectl/jsonpath/), which is a query language you can use to print specific fields. This is great for automation, when you want to query some details in a script:

```
kubectl get node <your-node> -o jsonpath='{.status.capacity.cpu}'

# e.g.
# kubectl get node docker-desktop -o jsonpath='{.status.capacity.cpu}'
```

> This tells you the number of CPU cores Kubernetes sees for that node.

📋 Can you run a similar command to show which container runtime your node is using?

<details>
  <summary>Not sure?</summary>

You can show all the fields by using `-o json` and then work out the query to see the details you want:

```
kubectl get node <node-name> -o jsonpath='{.status.nodeInfo.containerRuntimeVersion}'
```

The output will show which version of Docker or containerd your node is running.

</details><br/>

## Lab

Every object in Kubernetes can have **labels** - they are key-value pairs used to record additional information about the object.

Nodes have a set of labels which are provisioned by the environment, including some standard ones which all Kubernetes platforms provide.

Use Kubectl to find labels for your node, which will show you (among other things) the CPU architecture and operating system it's using.

> Stuck? Try [hints](hints.md) or check the [solution](solution.md).

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


