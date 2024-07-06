### Commands to remember

- Create Cluster based on file
```bash
kind create cluster --name madr-cluster --config=./k8s/kind.yaml
```

- Apply pod configuration to the cluster
```bash
kubectl apply -f ./k8s/application-pod.yaml
```

- Delete pod configuration from the cluster
```bash
kubectl delete -f ./k8s/application-pod.yaml
```
