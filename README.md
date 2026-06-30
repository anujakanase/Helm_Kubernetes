# Helm Charts for Payments & Shipping Services

This repository contains Helm charts for deploying the **Payments** and **Shipping** microservices on a Kubernetes cluster.

## Repository Structure

```
helm-repo/
├── payments/
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
│
├── shipping/
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
│
└── README.md
```

---

# What is Helm?

Helm is the **package manager for Kubernetes**.

Just as:

- apt installs packages on Ubuntu
- yum installs packages on CentOS
- npm installs Node.js packages

Helm installs and manages applications on Kubernetes.

A Helm package is called a **Chart**.

Helm simplifies Kubernetes deployments by packaging all required Kubernetes manifests into a reusable chart.

---

# Why Helm?

Without Helm, deploying an application requires creating and managing multiple YAML files manually.

Example:

- Deployment
- Service
- ConfigMap
- Secret
- Ingress
- PersistentVolume
- HorizontalPodAutoscaler

Managing these individually becomes difficult as applications grow.

Helm solves this by:

- Packaging Kubernetes resources together
- Supporting reusable templates
- Allowing environment-specific configurations
- Managing application versions
- Enabling easy upgrades and rollbacks

---

# What is a Helm Chart?

A Helm Chart is a collection of files that describe a Kubernetes application.

Typical structure:

```
mychart/
│
├── Chart.yaml
├── values.yaml
├── charts/
└── templates/
```

---

# Helm Components

## 1. Chart.yaml

Contains metadata about the chart.

Example:

```yaml
apiVersion: v2
name: payments
description: Helm chart for Payments Service
version: 0.1.0
appVersion: "1.0"
```

---

## 2. values.yaml

Stores configurable values used by templates.

Example:

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: latest

service:
  type: ClusterIP
  port: 80
```

---

## 3. templates/

Contains Kubernetes manifest templates.

Examples:

- deployment.yaml
- service.yaml
- ingress.yaml
- configmap.yaml
- secret.yaml

Helm replaces template variables using values from `values.yaml`.

Example:

```yaml
replicas: {{ .Values.replicaCount }}
```

---

## 4. Charts/

Stores dependent Helm charts.

Example:

```
charts/
└── redis/
```

---

## 5. templates/_helpers.tpl

Contains reusable template functions.

Example:

```yaml
{{ include "payments.fullname" . }}
```

---

# Helm Architecture

```
                Developer
                    │
                    │
             helm install
                    │
                    ▼
              Helm CLI
                    │
                    ▼
        Kubernetes API Server
                    │
                    ▼
          Kubernetes Cluster
                    │
        ┌───────────┴────────────┐
        │                        │
    Deployment               Service
        │                        │
        ▼                        ▼
      Pods                  Networking
```

---

# How Helm Communicates with Kubernetes

1. User executes a Helm command.

```
helm install payments ./payments
```

2. Helm reads:

- Chart.yaml
- values.yaml
- templates/

3. Helm renders templates.

Example:

```
{{ .Values.image.repository }}
```

becomes

```
nginx
```

4. Helm generates Kubernetes YAML manifests.

5. Helm sends the manifests to the Kubernetes API Server.

6. Kubernetes stores them in etcd.

7. Kubernetes controllers create the required resources.

- Pods
- Services
- ReplicaSets
- Deployments

---

# Helm Workflow

```
Developer
    │
    ▼
helm install
    │
    ▼
Read Chart
    │
    ▼
Merge values.yaml
    │
    ▼
Render Templates
    │
    ▼
Generate Kubernetes YAML
    │
    ▼
Kubernetes API Server
    │
    ▼
Deploy Resources
```

---

# Helm Release

Every installation of a Helm chart is called a **Release**.

Example:

```
helm install payments ./payments
```

Release Name:

```
payments
```

Commands:

```bash
helm list
```

```bash
helm status payments
```

```bash
helm uninstall payments
```

---

# Common Helm Commands

### Create Chart

```bash
helm create payments
```

### Install Chart

```bash
helm install payments ./payments
```

### Upgrade Chart

```bash
helm upgrade payments ./payments
```

### Rollback

```bash
helm rollback payments 1
```

### Uninstall

```bash
helm uninstall payments
```

### List Releases

```bash
helm list
```

### Render Templates

```bash
helm template payments ./payments
```

### Validate Chart

```bash
helm lint ./payments
```

---

# Benefits of Helm

- Reusable deployments
- Parameterized configuration
- Version control for deployments
- Easy upgrades
- Easy rollback
- Environment-specific configuration
- Dependency management
- Simplified Kubernetes application deployment

---

# Project Overview

This repository demonstrates how Helm can be used to package and deploy microservices.

Included Helm Charts:

- Payments Service
- Shipping Service

Each chart contains:

- Deployment
- Service
- Configurable values
- Templates
- Metadata

---

# Author

Created as part of Kubernetes & Helm learning to demonstrate Helm chart creation, templating, and deployment of microservices.
