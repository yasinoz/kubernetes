# Docker Images Used by Lab

This document provides a comprehensive mapping of Docker images used in each lab directory.

## Summary of All Unique Images

The following Docker images are used across all labs:

### Official/Third-Party Images
- `ghcr.io/yasinoz/library/adminer:4-standalone` - Database management UI
- `ghcr.io/yasinoz/library/alpine:3.24`, `ghcr.io/yasinoz/library/alpine:3.24` - Base Alpine Linux images
- `ghcr.io/yasinoz/library/busybox:1.34.0` - Lightweight utilities
- `ghcr.io/yasinoz/fluent/fluent-bit:1.8.3` - Log forwarding
- `gcr.io/cadvisor/cadvisor:v0.45.0` - Container metrics
- `ghcr.io/dexidp/dex:v2.44.0` - OpenID Connect provider
- `ghcr.io/yasinoz/moby/buildkit:v0.9.0` - Docker build toolkit
- `ghcr.io/yasinoz/library/nginx:1.18-alpine`, `ghcr.io/yasinoz/library/nginx:1.20-alpine`, `ghcr.io/yasinoz/library/nginx:1.23-alpine` - Web server
- `ghcr.io/yasinoz/openpolicyagent/gatekeeper:v3.23.1` - Policy enforcement
- `ghcr.io/yasinoz/library/percona:5.7.35` - MySQL variant
- `quay.io/argoproj/argocd:v3.5.5` - GitOps tool
- `quay.io/jetstack/cert-manager-cainjector:v1.5.3` - Certificate management
- `quay.io/jetstack/cert-manager-controller:v1.5.3` - Certificate management
- `quay.io/jetstack/cert-manager-webhook:v1.5.3` - Certificate management
- `quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.33.0` - Ingress controller
- `redis:8.10.2-alpine`, `redis:7-alpine` - In-memory data store

### Kubernetes Official Images
- `k8s.gcr.io/ingress-nginx/controller:v1.2.0` - Nginx ingress controller
- `k8s.gcr.io/kube-state-metrics/kube-state-metrics:v2.2.0` - Cluster metrics
- `k8s.gcr.io/metrics-server-amd64:v0.3.6` - Resource metrics
- `k8s.gcr.io/metrics-server/metrics-server:v0.6.1` - Resource metrics
- `ghcr.io/yasinoz/kubernetesui/dashboard:v2.3.1` - Kubernetes dashboard
- `ghcr.io/yasinoz/kubernetesui/metrics-scraper:v1.0.6` - Dashboard metrics
- `registry.k8s.io/metrics-server/metrics-server:v0.8.0` - Resource metrics

### Calico Network Policy Images
- `calico/cni:v3.15.0`
- `calico/kube-controllers:v3.15.0`
- `calico/node:v3.15.0`
- `calico/pod2daemon-flexvol:v3.15.0`

### Course/Tutorial Images
- `ghcr.io/yasinoz/courselabs/bad-sleep:latest` - Demonstration of failed container
- `docker.elastic.co/elasticsearch/elasticsearch:9.5.5` - Search engine
- `ghcr.io/yasinoz/courselabs/fulfilment-api:sha-ee65224fb578` - Demo API
- `ghcr.io/yasinoz/courselabs/fulfilment-processor:sha-578bc38424a1` - Demo processor
- `ghcr.io/yasinoz/grafana/grafana:13.2.4` - Metrics visualization
- `courselabs/keda-web:latest` - KEDA demo web app
- `courselabs/keda-worker:latest` - KEDA demo worker
- `docker.elastic.co/kibana/kibana:9.5.5` - Log visualization
- `ghcr.io/yasinoz/courselabs/nats-operator:v0.8.3` - NATS messaging operator
- `ghcr.io/yasinoz/prom/prometheus:v3.15.0` - Metrics collection
- `ghcr.io/yasinoz/diamol/ch02-hello-diamol-web:latest` - Demo web app
- `ghcr.io/yasinoz/kiamol/ch03-numbers-api:latest` - Demo API
- `ghcr.io/yasinoz/kiamol/ch03-numbers-web:latest` - Demo web app
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container for testing
- `ghcr.io/yasinoz/kiamol/ch05-pi:latest` - Pi calculation app
- `kiamol/ch05-pi-app` - Pi calculation app variant
- `ghcr.io/yasinoz/kiamol/ch09-vweb:v1`, `v2`, `v3` - Versioned web app
- `ghcr.io/yasinoz/kiamol/ch11-gogs:latest` - Git server
- `ghcr.io/yasinoz/kiamol/ch11-jenkins:2.319.1` - CI/CD server
- `ghcr.io/yasinoz/kiamol/ch14-access-log:latest` - Access log service
- `ghcr.io/yasinoz/kiamol/ch14-image-gallery:latest` - Image gallery app
- `ghcr.io/yasinoz/kiamol/ch14-image-of-the-day:latest` - Image API
- `ghcr.io/yasinoz/kiamol/ch15-cert-generator:latest` - Certificate generation
- `ghcr.io/yasinoz/kiamol/ch16-admission-webhook:latest` - Admission webhook
- `ghcr.io/yasinoz/kiamol/ch17-kube-explorer:latest` - Kubernetes explorer
- `ghcr.io/yasinoz/kiamol/ch17-user-cert-generator:latest` - User certificate generator
- `ghcr.io/yasinoz/kiamol/ch20-todo-list:latest` - Todo list app
- `ghcr.io/yasinoz/kiamol/ch20-todo-save-handler:latest` - Todo save handler
- `ghcr.io/yasinoz/sixeyed/configurable:21.04` - Configurable demo app
- `ghcr.io/yasinoz/sixeyed/kubectl:1.21.0` - Kubectl tool
- `ghcr.io/yasinoz/sixeyed/whoami:24.07`, `ghcr.io/yasinoz/sixeyed/whoami:24.07` - Identity service
- `ghcr.io/yasinoz/widgetario/products-db:postgres` - Products database
- `ghcr.io/yasinoz/widgetario/products-db:postgres-replicated` - Replicated products database

### Template Variables
- `access-log` - Template variable
- `image-gallery` - Template variable
- `image-of-the-day` - Template variable

## Images by Lab

### admission
- `ghcr.io/yasinoz/kiamol/ch05-pi:latest` - Pi calculation demo
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container
- `ghcr.io/yasinoz/kiamol/ch14-access-log:latest` - Access logging
- `ghcr.io/yasinoz/kiamol/ch14-image-gallery:latest` - Image gallery
- `ghcr.io/yasinoz/kiamol/ch14-image-of-the-day:latest` - Image API
- `ghcr.io/yasinoz/kiamol/ch16-admission-webhook:latest` - Admission webhook
- `ghcr.io/yasinoz/openpolicyagent/gatekeeper:v3.23.1` - Policy enforcement
- `quay.io/jetstack/cert-manager-*:v1.5.3` - Certificate management
- `ghcr.io/yasinoz/sixeyed/whoami:24.07` - Identity service

### affinity
- `ghcr.io/yasinoz/diamol/ch02-hello-diamol-web:latest` - Multi-arch demo
- `ghcr.io/yasinoz/sixeyed/whoami:24.07` - Identity service

### argo
- `ghcr.io/dexidp/dex:v2.44.0` - OIDC provider
- `ghcr.io/yasinoz/kiamol/ch11-gogs:latest` - Git server
- `quay.io/argoproj/argocd:v3.5.5` - ArgoCD
- `redis:8.10.2-alpine` - Redis cache
- Template variables: `access-log`, `image-gallery`, `image-of-the-day`

### buildkit
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container
- `ghcr.io/yasinoz/moby/buildkit:v0.9.0` - Build toolkit

### clusters
- `quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.33.0` - Ingress controller
- `ghcr.io/yasinoz/sixeyed/whoami:24.07` - Identity service

### configmaps
- `ghcr.io/yasinoz/sixeyed/configurable:21.04` - Configurable app demonstrating ConfigMaps

### daemonsets
- `ghcr.io/yasinoz/library/nginx:1.18-alpine` - Web server
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container

### deployments
- `ghcr.io/yasinoz/sixeyed/whoami:24.07` - Identity service for deployment demos

### docker
- No Kubernetes manifests (Docker-only lab)

### helm
- No fixed images (uses Helm templates)

### ingress
- `ghcr.io/yasinoz/kiamol/ch05-pi:latest` - Pi calculation
- `ghcr.io/yasinoz/library/nginx:1.18-alpine` - Default backend
- `k8s.gcr.io/ingress-nginx/controller:v1.2.0` - Ingress controller
- `ghcr.io/yasinoz/kiamol/ch15-cert-generator:latest` - TLS certificate generation
- `ghcr.io/yasinoz/sixeyed/configurable:21.04` - Configurable app
- `ghcr.io/yasinoz/sixeyed/whoami:24.07` - Identity service

### jenkins
- `ghcr.io/yasinoz/kiamol/ch11-gogs:latest` - Git server
- `ghcr.io/yasinoz/kiamol/ch11-jenkins:2.319.1` - Jenkins CI/CD
- `ghcr.io/yasinoz/moby/buildkit:v0.9.0` - Build toolkit

### jobs
- `ghcr.io/yasinoz/library/alpine:3.24` - Base image for jobs
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container
- `ghcr.io/yasinoz/kiamol/ch05-pi:latest` - Pi calculation
- `ghcr.io/yasinoz/sixeyed/kubectl:1.21.0` - Kubectl for CronJobs

### keda
- `courselabs/keda-web:latest` - KEDA demo web app
- `courselabs/keda-worker:latest` - KEDA demo worker
- `redis:7-alpine` - Redis queue
- `registry.k8s.io/metrics-server/metrics-server:v0.8.0` - Metrics server

### logging
- `docker.elastic.co/elasticsearch/elasticsearch:9.5.5` - Log storage
- `docker.elastic.co/kibana/kibana:9.5.5` - Log visualization
- `ghcr.io/yasinoz/library/alpine:3.24` - Sidecar container
- `ghcr.io/yasinoz/courselabs/fulfilment-api:sha-ee65224fb578` - Demo API
- `ghcr.io/yasinoz/courselabs/fulfilment-processor:sha-578bc38424a1` - Demo processor
- `ghcr.io/yasinoz/fluent/fluent-bit:1.8.3` - Log forwarding
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container

### monitoring
- `ghcr.io/yasinoz/grafana/grafana:13.2.4` - Metrics visualization
- `k8s.gcr.io/kube-state-metrics/kube-state-metrics:v2.2.0` - Cluster metrics
- `ghcr.io/yasinoz/courselabs/fulfilment-processor:sha-578bc38424a1` - Demo app
- `ghcr.io/yasinoz/prom/prometheus:v3.15.0` - Metrics collection
- `gcr.io/cadvisor/cadvisor:v0.45.0` - Container metrics

### namespaces
- `ghcr.io/yasinoz/kiamol/ch05-pi:latest` - Pi calculation
- `ghcr.io/yasinoz/library/nginx:1.18-alpine` - Web server
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container
- `ghcr.io/yasinoz/sixeyed/configurable:21.04` - Configurable app
- `ghcr.io/yasinoz/sixeyed/whoami:24.07` - Identity service

### networkpolicy
- Calico CNI images (`calico/*:v3.15.0`)
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container
- `ghcr.io/yasinoz/kiamol/ch14-access-log:latest` - Access logging
- `ghcr.io/yasinoz/kiamol/ch14-image-gallery:latest` - Image gallery
- `ghcr.io/yasinoz/kiamol/ch14-image-of-the-day:latest` - Image API

### nodes
- No container images (node management lab)

### operators
- `ghcr.io/yasinoz/library/busybox:1.34.0` - Init container
- `ghcr.io/yasinoz/courselabs/nats-operator:v0.8.3` - NATS operator
- MySQL operator images (`docker.io/bitpoke/mysql-operator*`)
- `ghcr.io/yasinoz/kiamol/ch20-todo-list:latest` - Todo app
- `ghcr.io/yasinoz/kiamol/ch20-todo-save-handler:latest` - Todo handler
- `ghcr.io/yasinoz/library/percona:5.7.35` - MySQL

### persistentvolumes
- `ghcr.io/yasinoz/kiamol/ch05-pi:latest` - Pi calculation
- `ghcr.io/yasinoz/library/nginx:1.18-alpine` - Web server with caching
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container

### pods
- `ghcr.io/yasinoz/courselabs/bad-sleep:latest` - Failing pod demo
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container
- `ghcr.io/yasinoz/sixeyed/whoami:24.07` - Identity service

### productionizing
- `ghcr.io/yasinoz/kiamol/ch05-pi:latest` - Pi calculation
- `k8s.gcr.io/metrics-server/metrics-server:v0.6.1` - Metrics for HPA
- `ghcr.io/yasinoz/sixeyed/configurable:21.04` - Configurable app
- `ghcr.io/yasinoz/sixeyed/whoami:24.07` - Identity service
- `ghcr.io/yasinoz/widgetario/products-db:postgres` - Database

### rbac
- `ghcr.io/yasinoz/kiamol/ch17-kube-explorer:latest` - Kubernetes explorer
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container
- `ghcr.io/yasinoz/kiamol/ch17-user-cert-generator:latest` - Certificate generator

### rollouts
- `ghcr.io/yasinoz/library/nginx:1.18-alpine`, `ghcr.io/yasinoz/library/nginx:1.20-alpine` - Different versions
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container
- `ghcr.io/yasinoz/kiamol/ch09-vweb:v1`, `v2`, `v3` - Versioned app

### secrets
- `ghcr.io/yasinoz/sixeyed/configurable:21.04` - Configurable app demonstrating secrets

### services
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container
- `ghcr.io/yasinoz/sixeyed/whoami:24.07` - Identity service

### statefulsets
- `ghcr.io/yasinoz/library/adminer:4-standalone` - Database UI
- `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` - Sleep container
- `ghcr.io/yasinoz/library/nginx:1.18-alpine` - Web server
- `ghcr.io/yasinoz/widgetario/products-db:postgres-replicated` - Replicated database

### tools
- `k8s.gcr.io/metrics-server-amd64:v0.3.6` - Metrics server
- `ghcr.io/yasinoz/kiamol/ch03-numbers-api:latest` - Demo API
- `ghcr.io/yasinoz/kiamol/ch03-numbers-web:latest` - Demo web app
- `ghcr.io/yasinoz/kubernetesui/dashboard:v2.3.1` - Kubernetes dashboard
- `ghcr.io/yasinoz/kubernetesui/metrics-scraper:v1.0.6` - Dashboard metrics

### troubleshooting
- `ghcr.io/yasinoz/kiamol/ch05-pi:latest` - Working version
- `kiamol/ch05-pi-app` - Broken version

### troubleshooting-2
- `ghcr.io/yasinoz/library/nginx:1.23-alpine` - Web server

### troubleshooting-3
- Uses Helm templates (no fixed images)

## Notes

1. Some labs use template variables (e.g., `{{ .Values.image }}`) which are resolved at deployment time
2. The `docker` and `helm` labs don't contain fixed image references in their specs
3. Many images are versioned (e.g., `:21.04`, `:v1.2.0`) for reproducibility
4. Several labs use the same images for consistency (e.g., `ghcr.io/yasinoz/kiamol/ch03-sleep:latest` for testing)
5. The course uses a mix of official images, course-specific images, and third-party images
