# Amazon EKS Cluster Setup

## Overview

This document explains how to create and delete an Amazon Elastic Kubernetes Service (EKS) cluster using `eksctl` with AWS Fargate.

## Prerequisites

Before creating the cluster, ensure the following tools are installed:

* AWS CLI
* eksctl
* kubectl

Configure AWS credentials:

```powershell
aws configure
```

Verify AWS access:

```powershell
aws sts get-caller-identity
```

---

# Create EKS Cluster using Fargate

Create an EKS cluster with AWS Fargate:

```powershell
.\eksctl.exe create cluster `
 --name demo-cluster `
 --region us-east-1 `
 --fargate
```

This creates:

* Amazon EKS control plane
* Fargate profile
* Kubernetes networking configuration
* Core Kubernetes components

---

# Verify Cluster

Check cluster status:

```powershell
.\eksctl.exe get cluster --region us-east-1
```

Update kubeconfig:

```powershell
aws eks update-kubeconfig `
 --name demo-cluster `
 --region us-east-1
```

Verify Kubernetes connection:

```powershell
kubectl get nodes
```

---

# Delete EKS Cluster

To avoid unnecessary AWS charges after testing:

```powershell
.\eksctl.exe delete cluster `
 --name demo-cluster `
 --region us-east-1
```

This removes:

* EKS cluster
* Fargate profiles
* AWS resources created by eksctl

---

# Result

Successfully created and managed an Amazon EKS cluster using AWS Fargate and eksctl.
