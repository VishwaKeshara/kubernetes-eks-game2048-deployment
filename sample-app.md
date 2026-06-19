# Sample Application Deployment on Amazon EKS

## Overview

This document explains how to deploy a sample NGINX application on Amazon EKS using Kubernetes Deployment and Service resources.

The deployment creates multiple replicas and exposes the application internally through a Kubernetes Service.

---

# 1. Create Deployment Manifest

Create a file:

```text
deploy.yaml
```

Add the following Kubernetes Deployment configuration:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: eks-sample-linux-deployment
  labels:
    app: eks-sample-linux-app

spec:
  replicas: 3

  selector:
    matchLabels:
      app: eks-sample-linux-app

  template:
    metadata:
      labels:
        app: eks-sample-linux-app

    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: kubernetes.io/arch
                operator: In
                values:
                - amd64
                - arm64

      containers:
      - name: nginx
        image: public.ecr.aws/nginx/nginx:1.23
        ports:
        - name: http
          containerPort: 80
        imagePullPolicy: IfNotPresent

      nodeSelector:
        kubernetes.io/os: linux
```

---

# 2. Deploy Application

Apply the deployment:

```bash
kubectl apply -f deploy.yaml
```

---

# 3. Verify Deployment

Check pods:

```bash
kubectl get pods
```

Expected:

```text
eks-sample-linux-deployment-xxxxx   Running
```

Check deployment:

```bash
kubectl get deployment
```

Expected:

```text
NAME                          READY
eks-sample-linux-deployment   3/3
```

---

# 4. Create Kubernetes Service

Create:

```text
service.yaml
```

Add:

```yaml
apiVersion: v1

kind: Service

metadata:
  name: eks-sample-linux-service
  labels:
    app: eks-sample-linux-app

spec:
  selector:
    app: eks-sample-linux-app

  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

---

# 5. Deploy Service

Apply service:

```bash
kubectl apply -f service.yaml
```

---

# 6. Verify Service

Check service:

```bash
kubectl get service
```

Example:

```text
NAME                         TYPE
eks-sample-linux-service    ClusterIP
```

---

# Deployment Flow

```
User
 |
Kubernetes Service
 |
Deployment
 |
NGINX Pods (3 replicas)
 |
Container Image
```

---

# Screenshots

## Running Pods

![Pods Running](../screenshots/sample-app-pods.png)

## Kubernetes Service

![Service](../screenshots/sample-app-service.png)
