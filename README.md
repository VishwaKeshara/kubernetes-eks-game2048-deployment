# Kubernetes End-to-End Deployment on Amazon EKS

## Overview

This project demonstrates deploying a containerized application on Amazon EKS using Kubernetes and AWS cloud-native services.

## Architecture

User
 |
AWS ALB
 |
Kubernetes Ingress
 |
Service
 |
Pods
 |
Containers


## Technologies

- AWS EKS
- Kubernetes
- Helm
- AWS Load Balancer Controller
- IAM OIDC
- Docker
- kubectl


## Implementation Steps

1. Created EKS cluster using eksctl
2. Configured IAM OIDC provider
3. Created IAM service account
4. Installed AWS Load Balancer Controller using Helm
5. Deployed application using Kubernetes manifests
6. Exposed application using AWS ALB


## Commands

Create cluster:

eksctl create cluster -f eks/cluster-config.yaml


Deploy application:

kubectl apply -f kubernetes/


Verify:

kubectl get pods
kubectl get ingress
