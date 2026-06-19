# AWS Load Balancer Controller Setup on Amazon EKS

## Overview

The AWS Load Balancer Controller manages AWS Application Load Balancers (ALB) for Kubernetes Ingress resources. This allows applications running inside Amazon EKS to be accessed externally through AWS ALB.

## 1. Download IAM Policy

Download the AWS Load Balancer Controller IAM policy:

```powershell
curl.exe -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json
```

---

## 2. Create IAM Policy

Create IAM policy required by the AWS Load Balancer Controller:

```powershell
aws iam create-policy `
 --policy-name AWSLoadBalancerControllerIAMPolicy `
 --policy-document file://iam_policy.json
```

Created policy:

```
AWSLoadBalancerControllerIAMPolicy
```

---

## 3. Create IAM Service Account (IRSA)

Associate IAM role with Kubernetes service account:

```powershell
.\eksctl.exe create iamserviceaccount `
 --cluster=demo-cluster `
 --namespace=kube-system `
 --name=aws-load-balancer-controller `
 --role-name=AmazonEKSLoadBalancerControllerRole `
 --attach-policy-arn=arn:aws:iam::288711195978:policy/AWSLoadBalancerControllerIAMPolicy `
 --approve
```

This enables Kubernetes pods to securely access AWS services using IAM Roles for Service Accounts (IRSA).

---

## 4. Install Helm Repository

Add AWS EKS Helm repository:

```powershell
.\helm.exe repo add eks https://aws.github.io/eks-charts
```

Update repository:

```powershell
.\helm.exe repo update
```

---

## 5. Install AWS Load Balancer Controller

Install controller using Helm:

```powershell
.\helm.exe install aws-load-balancer-controller eks/aws-load-balancer-controller `
 -n kube-system `
 --set clusterName=demo-cluster `
 --set serviceAccount.create=false `
 --set serviceAccount.name=aws-load-balancer-controller `
 --set region=us-east-1 `
 --set vpcId=vpc-02999305b9e7fd9b8
```

---

## 6. Verify Deployment

Check controller status:

```powershell
kubectl get deployment -n kube-system aws-load-balancer-controller
```

Expected output:

```
NAME                           READY
aws-load-balancer-controller   2/2
```

---

## Result

AWS Load Balancer Controller was successfully deployed on Amazon EKS and configured to create AWS Application Load Balancers through Kubernetes Ingress resources.

Screenshot:

![AWS Load Balancer Controller](../screenshots/alb-controller.png)
