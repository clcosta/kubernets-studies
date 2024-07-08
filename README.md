### Commands to remember

- Create Cluster based on file

```bash
kind create cluster --name madr-cluster --config=./k8s/kind.yaml
```

- Apply configuration based on file to cluster

```bash
kubectl apply -f ./k8s/application-pod.yaml
```

- Delete configuration based on file

```bash
kubectl delete -f ./k8s/application-pod.yaml
```

### Rollback Deployment

- Get the deployment history

```bash
kubectl rollout history deployment madr
```

- Rollback to a specific revision

```bash
kubectl rollout undo deployment madr
```

Or  

```bash
kubectl rollout undo deployment madr --to-revision=1
```


The same command apply are used to **_replicasets_**, **_deployments_**, **_services_** and **_ingress_**.

### Hiearchy of Kubernetes

1. Cluster
2. Node
3. Pod
4. Container  
---
1. Deployment
2. ReplicaSet
3. Pod
