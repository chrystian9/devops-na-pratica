# Replica Set

## 1 - Criar arquivo my-replicaset.yaml

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
  
  labels:
    app: my-replicaset
    
spec:
  replicas: 5
  selector:
    matchLabels:
      app: my-replicaset
      
  template:
    metadata:
      labels:
        app: my-replicaset
        
    spec:
      containers:
      - name: my-replicaset

        image: nginx:latest
        ports:
        - name: web
          containerPort: 80
          protocol: TCP
```

## 2 - Aplicar replica set

```
kubectl apply -f .\my-replicaset.yaml
```

### Resultado:
```
replicaset.apps/my-replicaset created
```

## 3 - Listando replica sets

```
kubectl get rs -A
```

### Resultado:

```
NAMESPACE     NAME                 DESIRED   CURRENT   READY   AGE
default       my-replicaset        5         5         0       2s
kube-system   coredns-5d78c9869d   2         2         2       24h
```

## 4 - Inspecionando replica set

```
kubectl describe rs my-replicaset
```

### Resultado: 

```
Name:         my-replicaset
Namespace:    default
Selector:     app=my-replicaset
Labels:       app=my-replicaset
Annotations:  <none>
Replicas:     5 current / 5 desired
Pods Status:  5 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=my-replicaset
  Containers:
   my-replicaset:
    Image:        nginx:latest
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:
  Type    Reason            Age   From                   Message
  ----    ------            ----  ----                   -------
  Normal  SuccessfulCreate  70s   replicaset-controller  Created pod: my-replicaset-sjcjr
  Normal  SuccessfulCreate  70s   replicaset-controller  Created pod: my-replicaset-t2dp5
  Normal  SuccessfulCreate  70s   replicaset-controller  Created pod: my-replicaset-bg5gg
  Normal  SuccessfulCreate  70s   replicaset-controller  Created pod: my-replicaset-nhsqj
  Normal  SuccessfulCreate  70s   replicaset-controller  Created pod: my-replicaset-5hl45
```

## 5 - Verificando pods na lista de pods do cluster

```
kubectl get pods -A
```

### Resultado:

```
NAMESPACE     NAME                                     READY   STATUS    RESTARTS      AGE
default       my-replicaset-5hl45                      1/1     Running   0             2m26s
default       my-replicaset-bg5gg                      1/1     Running   0             2m26s
default       my-replicaset-nhsqj                      1/1     Running   0             2m26s
default       my-replicaset-sjcjr                      1/1     Running   0             2m26s
default       my-replicaset-t2dp5                      1/1     Running   0             2m26s
kube-system   coredns-5d78c9869d-jm4sv                 1/1     Running   1 (22m ago)   24h
kube-system   coredns-5d78c9869d-nlmpq                 1/1     Running   1 (22m ago)   24h
kube-system   etcd-docker-desktop                      1/1     Running   1 (22m ago)   24h
kube-system   kube-apiserver-docker-desktop            1/1     Running   1 (22m ago)   24h
kube-system   kube-controller-manager-docker-desktop   1/1     Running   8 (22m ago)   24h
kube-system   kube-proxy-5kstf                         1/1     Running   1 (22m ago)   24h
kube-system   kube-scheduler-docker-desktop            1/1     Running   5 (22m ago)   24h
kube-system   storage-provisioner                      1/1     Running   5 (22m ago)   24h
kube-system   vpnkit-controller                        1/1     Running   1 (22m ago)   24h
```

## 6 - Deletar replica set

```
kubectl delete pod my-first-pod
```

### Resultado:

```
replicaset.apps "my-replicaset" deleted
```

### Lista de replica sets após delete

```
NAMESPACE     NAME                 DESIRED   CURRENT   READY   AGE
kube-system   coredns-5d78c9869d   2         2         2       24h
```