# StatefulSets in Kubernetes

Issues with Deployments & Stateful Applications:
Note: Persistent volumes in k8s can be (attached to host or external provider)

- Pods share the same persistent volumes
- Pods dont have a persistent ID
- Pods dont have DNS names, so they cant be addressed individually
- Pods cant handling delicate scaling

Statefulsets & Stateful Applications:

- Pods get their own DNS
- Pods have persistent ID
- Each Pods get their own Persistent Volume
- Delicate scaling (graceful scale up & down while retaining persistsent volume)

Notes:

- K8s stateful sets do not solve the replication problem (engineers still need to know how stateful apps work e.g. Redis)
- Need to know what persistent storage type to use

## Create a namespace

```
kubectl create ns example
```

## Check the storageclass for host path provisioner

```
kubectl get storageclass
```

## Deploy our statefulset

```
kubectl -n example apply -f .\statefulsets\statefulset.yaml
kubectl -n example apply -f .\statefulsets\example-app.yaml
```

## Enable Redis Cluster

```
$IPs = $(kubectl -n example get pods -l app=redis-cluster -o jsonpath='{range.items[*]}{.status.podIP}:6379 ')
kubectl -n example exec -it redis-cluster-0  -- /bin/sh -c "redis-cli -h 127.0.0.1 -p 6379 --cluster create ${IPs}"
kubectl -n example exec -it redis-cluster-0  -- /bin/sh -c "redis-cli -h 127.0.0.1 -p 6379 cluster info"
```

## More info

https://rancher.com/blog/2019/deploying-redis-cluster

## Simulate redis replication feature

Notice counter state is still being incremented via browser (localhost)

```
#scale down
kubectl -n example scale statefulset redis-cluster --replicas 4

or

# delete some pods
kubectl -n example delete pods redis-cluster-0 redis-cluster-4

# notice PVs are still available
kubectl -n example get pv
```
