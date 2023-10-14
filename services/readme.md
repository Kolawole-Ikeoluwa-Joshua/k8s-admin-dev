# Load Balancing & Service Discovery:

```
# Namespaces
create a namespace and deploy pods into it

# Service
supply selector on service and supply label selector on pods via deployments

# Services behind the scene:
- docker / container runtime
- linux bridge - subnet (ifconfig or brctl show)
- coredns
- kernel ip tables (kube-proxy manages ip tables)

kubectl get pods --show-labels
kubectl get service

kubectl apply -f <service>
```
