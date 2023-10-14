# Getting started

Kubernetes Development Guide

## Config

```
kubectl config

${HOME}/.kube/config
kubectl config --kubeconfig="C:\someotherfolder\config"
$KUBECONFIG
```

## Context

```
# view current cluster (context)
kubectl config current-context

# change cluster
kubectl config get-contexts
kubectl config use-context
```

## GET commands

```
kubectl get <resources>

#examples
kubectl get services
kubectl get ingress
```

## Namespaces

```
kubectl get namespaces
kubectl create namespace test
kubectl get pods -n test
```

## Describe command

```
# troubleshoot states and statuses of objects

kubectl describe <resource> <name>

kubectl describe pod coredns-565d847f94-bnvjl -n kube-system
```

## Version

```
kubectl version
```

**Virtual Tutor:** [Marcel-Dempers](https://github.com/marcel-dempers)

# Advanced Labs:

- [Kubernetes Deployments](deployments/readme.md)
- [Kubernetes Configuration Management](configmap/readme.md)
- [Kubernetes Secret Management](secrets/readme.md)
- [Load Balancing & Service Discovery](services/readme.md)
- [Kubernetes Ingress](ingress/readme.md)
- [StatefulSets in Kubernetes](statefulsets/notes.md)
- [Persistent Volumes on Kubernetes](persistentvolume/readme.md)
- [Kubernetes Daemonsets](daemonsets/README.md)
- [Kubernetes & Kustomize](kustomize/readme.md)
- [Kubernetes in the Cloud: Terraform + EKS](iac/terraform/readme.md)
- [Kubernetes & Helm](helm/README.md)
- [Kubernetes RBAC](rbac/README.md)
