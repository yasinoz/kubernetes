---
title: ConfigMaps
lang: en
slug: configmaps
section: modelling
order: 1
duration: 40
requires: [kubernetes]
status: updated
upstream: labs/configmaps
---
# Configuring Apps with ConfigMaps

There are two ways to store configuration settings in [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/) - either as key-value pairs, which you'll surface as environment variables, or as text data which you'll surface as files in the container filesystem.

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

> Run every command from the **root of this repository** - the paths in this lab are written relative to it.

## API specs

- [ConfigMap](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/#configmap-v1-core)

<details>
  <summary>YAML overview</summary>

## ConfigMap and Pod YAML - using environment variables

Key-value pairs are defined in YAML like this:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: configurable-env
data:
  Configurable__Environment: uat
```

The metadata is standard - you'll reference the name of the ConfigMap in the Pod spec to load settings.

* `data` - list of settings as key-value pairs, separated with colons

In the Pod spec you add a reference:

```
spec:
  containers:
    - name: app
      image: ghcr.io/yasinoz/sixeyed/configurable:21.04
      envFrom:
        - configMapRef:
            name: configurable-env
```

* `envFrom` - load all the values in the source as environment variables

## ConfigMap and Pod YAML - using the container filesystem

Text files are defined in the same YAML structure, with an entry for each file:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: configurable-override
data:
  override.json: |-
    {
      "Configurable": {
        "Release": "21.04.01"
      }
    }
```

> Careful with the whitespace - the file data needs to be indented one stop more than the filename

The API spec is the same, but in this format:

* `data` - list of files, with the filename set and the contents following the separator `|-`

In the Pod spec you can load all the values into the container filesystem as volume mounts:

```
spec:
  containers:
    - name: app
      image: ghcr.io/yasinoz/sixeyed/configurable:21.04
      volumeMounts:
        - name: config-override
          mountPath: "/app/config"
          readOnly: true
  volumes:
    - name: config-override
      configMap:
        name: configurable-override
```

Volumes are defined at the Pod level - they are storage units which are part of the Pod environment. You load the storage unit into the container filesystem using a mount.

* `volumes` - list of storage units to load, can be ConfigMaps, Secrets or other types
* `volumeMounts` - list of volumes to mount into the container filesystem
* `volumeMounts.name` - matches the name of the volume
* `volumeMounts.mountPath` - the directory path where the volume is surfaced
* `volumeMounts.readOnly` - flag whether the volume is read-only or editable

</details><br/>

## Run the configurable demo app

The demo app for this lab has the logic to merge config from multiple sources. 

Defaults are built into the `appsettings.json` file inside the Docker image - run a Pod with no config applied to see the defaults:

```
kubectl run configurable --image=ghcr.io/yasinoz/sixeyed/configurable:21.04 --labels='kubernetes.courselabs.co=configmaps'

kubectl wait --for=condition=Ready pod configurable

kubectl port-forward pod/configurable 8080:80
```

> These are useful commands for quick testing or debugging, but in real life it's all YAML

Check the app at http://localhost:8080 (or your node's IP address if you have a remote cluster).

You see the default configuration settings from the JSON file in the container image. The environment variables come from Dockerfile, plus the container OS and some set by Kubernetes.

📋 Exit the port-forward and remove the Pod.

<details>
  <summary>Not sure how?</summary>

```
# Ctrl-C to exit the command

kubectl delete pod configurable
```

</details><br />

## Setting config with environment variables in the Pod spec

The Pod spec is where you apply configuration:

- [deployment.yaml](specs/configurable/deployment.yaml) adds a config setting with an environment variable in the template Pod spec.

📋 Deploy the app from the folder `labs/configmaps/specs/configurable/`

<details>
  <summary>Not sure how?</summary>

```
kubectl apply -f labs/configmaps/specs/configurable/
```

</details><br />

You can check the environment variable is set by running `printenv` inside the Pod container:

```
kubectl exec deploy/configurable -- printenv | grep __
```

> You should see `Configurable__Release=24.01.1`

📋 Confirm that by browsing to the app from your Service.

<details>
  <summary>Not sure how?</summary>

```
# print the Service details:
kubectl get svc -l app=configurable
```

</details><br />

## Setting config with environment variables in ConfigMaps

Environment variables in Pod specs are fine for single settings like feature flags. Typically you'll have lots of settings and you'll use a ConfigMap:

- [configmap-env.yaml](specs/configurable/config-env/configmap-env.yaml) - a ConfigMap with multiple environment variables
- [deployment-env.yaml](specs/configurable/config-env/deployment-env.yaml) - a Deployment which loads the ConfigMap into environment variables

```
kubectl apply -f labs/configmaps/specs/configurable/config-env/
```

> This creates a new ConfigMap and updates the Deployment. Remember which object the Deployment uses to manage Pods?

📋 Print the environment variables set in the updated Pod.

<details>
  <summary>Not sure how?</summary>

```
kubectl exec deploy/configurable -- printenv | grep __
```

</details><br />

> You should see the release is now `24.01.2` and there's a new setting `Configurable__Environment=uat`

## Setting config with files in ConfigMaps

Environment variables are limited too. They're visible to all processes so there's a potential to leak sensitive information. There can also be collisions if the same keys are used by different processes.

The filesystem is a more reliable store for configuration; permissions can be set for files, and it allows for more complex config with nested settings.

The demo app can use JSON configuration as well as environment variables, and it supports loading additional settings from an override file:

- [configmap-json.yaml](specs/configurable/config-json/configmap-json.yaml) - stores the config settings as a JSON data item
- [deployment-json.yaml](specs/configurable/config-json/deployment-json.yaml) loads the JSON as a volume mount **and** loads environment variables

```
kubectl apply -f labs/configmaps/specs/configurable/config-json/
```

> Refresh the web app and you'll see new settings coming from the `config/override.json` file

📋 Check the filesystem inside the container to see the file loaded from the ConfigMap into the `/app/config` path.

<details>
  <summary>Not sure how?</summary>

Explore the container filesystem with `exec` commands:

```
kubectl exec deploy/configurable -- ls /app/

kubectl exec deploy/configurable -- ls /app/config/

kubectl exec deploy/configurable -- cat /app/config/override.json
```

> The first JSON file is from the container image, the second is from the ConfigMap volume mount.

</details><br />

Something's not quite right though - the release setting is still coming from the environment variable:

```
kubectl exec deploy/configurable -- cat /app/config/override.json

kubectl exec deploy/configurable -- printenv | grep __
```

> The config hierarchy in this app puts environment variables ahead of settings in files, so they get overidden. You'll need to understand the hierarchy for your apps to model config correctly.

## Lab

Mapping configuration in ConfigMap YAML works well and it means you can deploy your whole app with `kubectl apply`. But it won't suit every organization, and Kubernetes also supports creating ConfigMaps directly from values and config files - without any YAML.

Create two new ConfigMaps to support the Deployment in [deployment-lab.yaml](specs/configurable/lab/deployment-lab.yaml) and set these values:

- Environment variable `Configuration__Release=21.04-lab`
- JSON setting `Features.DarkMode=true`

> Stuck? Try [hints](hints.md) or check the [solution](solution.md).

___
## **EXTRA** Be careful with volume mounts

<details>
  <summary>Volume mounts can overwrite existing directories </summary>

Loading ConfigMaps into volume mounts is very powerful, but there are a couple of gotchas to be aware of:

1. Updating the ConfigMap does not trigger a Pod replacement; the new file contents are loaded into the volume mount, but the app in the container may ignore that if it only reads config files at startup;

2. Volumes are not merged into the target path for a volume mount - if the directory already exists, the volume mount **replaces it** with the contents of the volume.

You can easily break your app if you get the volume mounts wrong:

- [deployment-broken.yaml](specs/configurable/config-broken/deployment-broken.yaml) - mounts a ConfigMap into the `/app` directorry, which overwrites the actual app folder from the image

```
kubectl apply -f labs/configmaps/specs/configurable/config-broken/

kubectl get pods -l app=configurable --watch
```

> A new Pod gets created, errors and goes into CrashLoopBackoff. 

```
# Ctrl-C

kubectl logs -l app=configurable,broken=bad-mount
```

> The mount replaces the entire app folder, so there is no application to run :)

But the original Pod doesn't get replaced:

```
kubectl get replicaset -l app=configurable 
```

> The Deployment object won't scale down the old ReplicaSet until the new one reaches desired capacity. Using a Deployment keeps your app safe from issues like this.

</details>

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

Cleanup by removing objects with this lab's label:

```
kubectl delete configmap,deploy,svc,pod -l kubernetes.courselabs.co=configmaps
```
