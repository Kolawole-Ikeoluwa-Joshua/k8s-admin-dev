# Kubernetes Ingress:

- SSL Termination
- Routing
- URL Routing/Rewrites
- Load Balancing
- Ingress: (TLS, Host or DNS, Path, Ports) examples: Nginx, HAProxy etc

```
kubectl create ns example-app
kubectl create ns <namespace>

kubectl apply -n <namespace> -f <k8s manifest>

kubectl get pods -n <namespace>

kubectl -n <namespace> port-forward <pod-name> <port>

# NGINX CONTROLLER:

# deploy namespace
kubectl apply -f <some-directory-path>\namespace.yaml

# deploy service account
kubectl apply -f <some-directory-path>\service-account.yaml

# deploy permissions for service account
kubectl apply -f <some-directory-path>\cluster-role.yaml

# bind roles to service account
kubectl apply -f <some-directory-path>\cluster-role-binding.yaml

# ingress configs (controls what ingress controller can do)
kubectl apply -f <some-directory-path>\configMap.yaml

# ingress controller deployment
kubectl apply -f <some-directory-path>\deployment.yaml

# loadbalancer service
kubectl apply -f <some-directory-path>\service.yaml

# tls certificate (always specific to a microservice)
# must deploy in microservice namespace
kubectl apply -f <some-directory-path>\tls-secret.yaml

# deploy a ingress manifest
kubectl apply -f <some-directory-path>\ingress-nginx-example.yaml
```

## Using helm templates to deploy ingress

- [Nginx & Helm](./controller/nginx/README.md)
