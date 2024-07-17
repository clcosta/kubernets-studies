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


### Proxy

- Create a proxy to api

```bash
kubectl proxy --port=8080
```

#### Acess the service using proxy

```bash
curl localhost:8080/api/v1/namespaces/:namespace/services/:service | jq
```
  - namespace: default
  - service: madr-service

### Services

### ClusterIp

Will create a simple service. Expose a internal IP to cluster.

Necessary port-forward to access the service.

```bash
kubectl port-forward service/madr-service 8080:8000
```

#### Node Port
**Constraint**
- 30000 > {Node Port} < 32767

Will open the port in all nodes

```
Node Port: 3001
---
Node 1 -> 30001
Node 2 -> 30001
Node 3 -> 30001
```

Use case:<br>

Access the port 30001 from inside node. Example: Node 1 acess Node2:30001. Bassically, the node port is open in all nodes and redirect to the service.

#### Load Balancer
Will create a Load Balancer and give a public IP to access the service.

**Only works in cloud providers**

Use Case:<br>

Expose any application to the internet in a public IP.


### Environment Variables

#### Disclaimer

In the **fastzero-madr** application. The variabels we can use are:

- DATABASE_URL : String
- AUTH_SECRET_KEY: String
- AUTH_ALGORITHM: String = HS256
- AUTH_EXPIRES_IN_MINUTES: Int = 60

In this case, we only variables i'll change are **DATABASE_URL** and **AUTH_SECRET_KEY**.

#### Interactive terminal

```bash
kubectl 'exec' '-it' :POD_NAME '--namespace' 'default' '--container' :CONTAINER_NAME '--' 'bash'
```

  - POD_NAME: madr-597d774c84-2p2x4
    **In this case madr is the name of deployment. The rest it's the replicaset and pod name.**
  - CONTAINER_NAME: madr

#### ConfigMap

The config map is a way to store the environment variables in a single place. 

The config map change the enviroment variables in the deployment.

```yaml
- name: DATABASE_URL
  valueFrom:
    configMapKeyRef:
      name: madr-env
      key: DATABASE_URL
```