# Simplify Kubernetes YAML with Kustomize

## The Basics

```

kubectl apply -f /kustomize/application/namespace.yaml
kubectl apply -f /kustomize/application/configmap.yaml
kubectl apply -f /kustomize/application/deployment.yaml
kubectl apply -f /kustomize/application/service.yaml

# OR

kubectl apply -f /kustomize/application/

kubectl delete ns example

```

## Kustomize

### Build

```
kubectl kustomize .\kustomize\ | kubectl apply -f -
# OR
kubectl apply -k .\kustomize\

kubectl delete ns example
```

### Overlays

```
kubectl kustomize .\kustomize\environments\production | kubectl apply -f -
# OR
kubectl apply -k .\kustomize\environments\production

kubectl delete ns example
```
