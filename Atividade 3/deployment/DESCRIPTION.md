# Deployment

## 1 - Criar arquivo my-deployment.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name:  my-deployment
  namespace: default
  labels:
    app:  my-deployment
spec:
  selector:
    matchLabels:
      app: my-deployment
  replicas: 4
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        app:  my-deployment
    spec:
      containers:
      - name:  my-deployment
        image: nginx:1.14.2
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
          limits:
            cpu: 100m
            memory: 100Mi
        livenessProbe:
          tcpSocket:
            port: 80
          initialDelaySeconds: 5
          timeoutSeconds: 5
          successThreshold: 1
          failureThreshold: 3
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /_status/healthz
            port: 80
          initialDelaySeconds: 5
          timeoutSeconds: 2
          successThreshold: 1
          failureThreshold: 3
          periodSeconds: 10
      restartPolicy: Always
```

## 2 - Aplicar replica set

```
kubectl apply -f .\my-deployment.yaml
```

### Resultado:
```
deployment.apps/my-deployment created
```

## 3 - Listando deployments

```
kubectl get deployments -A
```

### Resultado:

```
NAMESPACE     NAME            READY   UP-TO-DATE   AVAILABLE   AGE
default       my-deployment   0/1     1            0           2m13s
kube-system   coredns         2/2     2            2           24h
```

## 4 - Inspecionando replica set

```
kubectl describe deployment my-deployment
```

### Resultado: 

```
Name:                   my-deployment
Namespace:              default
CreationTimestamp:      Wed, 04 Oct 2023 17:49:46 -0300
Labels:                 app=my-deployment
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=my-deployment
Replicas:               4 desired | 4 updated | 4 total | 0 available | 4 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=my-deployment
  Containers:
   my-deployment:
    Image:      nginx:1.14.2
    Port:       <none>
    Host Port:  <none>
    Limits:
      cpu:     100m
      memory:  100Mi
    Requests:
      cpu:        100m
      memory:     100Mi
    Liveness:     tcp-socket :80 delay=5s timeout=5s period=10s #success=1 #failure=3
    Readiness:    http-get http://:80/_status/healthz delay=5s timeout=2s period=10s #success=1 #failure=3    
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      False   MinimumReplicasUnavailable
  Progressing    True    ReplicaSetUpdated
OldReplicaSets:  <none>
NewReplicaSet:   my-deployment-6c797df6c6 (4/4 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  7s    deployment-controller  Scaled up replica set my-deployment-6c797df6c6 to 4
```

## 5 - Verificando lista de pods

```
kubectl get pods -A
```

### Resultado:

```
NAMESPACE     NAME                                     READY   STATUS    RESTARTS      AGE
default       my-deployment-6c797df6c6-2mts2           0/1     Running   0             41s
default       my-deployment-6c797df6c6-5vw98           0/1     Running   0             41s
default       my-deployment-6c797df6c6-7m6f6           0/1     Running   0             41s
default       my-deployment-6c797df6c6-vpjct           0/1     Running   0             41s
kube-system   coredns-5d78c9869d-jm4sv                 1/1     Running   1 (38m ago)   24h
kube-system   coredns-5d78c9869d-nlmpq                 1/1     Running   1 (38m ago)   24h
kube-system   etcd-docker-desktop                      1/1     Running   1 (38m ago)   24h
kube-system   kube-apiserver-docker-desktop            1/1     Running   1 (38m ago)   24h
kube-system   kube-controller-manager-docker-desktop   1/1     Running   8 (38m ago)   24h
kube-system   kube-proxy-5kstf                         1/1     Running   1 (38m ago)   24h
kube-system   kube-scheduler-docker-desktop            1/1     Running   5 (38m ago)   24h
kube-system   storage-provisioner                      1/1     Running   5 (38m ago)   24h
kube-system   vpnkit-controller                        1/1     Running   1 (38m ago)   24h
```

## 5 - Atualiza versão da imagem nginx

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name:  my-deployment
  namespace: default
  labels:
    app:  my-deployment
spec:
  selector:
    matchLabels:
      app: my-deployment
  replicas: 4
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        app:  my-deployment
    spec:
      containers:
      - name:  my-deployment
        image: nginx:1.16.1
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
          limits:
            cpu: 100m
            memory: 100Mi
        livenessProbe:
          tcpSocket:
            port: 80
          initialDelaySeconds: 5
          timeoutSeconds: 5
          successThreshold: 1
          failureThreshold: 3
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /_status/healthz
            port: 80
          initialDelaySeconds: 5
          timeoutSeconds: 2
          successThreshold: 1
          failureThreshold: 3
          periodSeconds: 10
      restartPolicy: Always
```

## 6 - Aplica novamente o deployment, realizando assim a atualização

```
kubectl apply -f .\my-deployment.yaml
```

### Resultado:
```
deployment.apps/my-deployment configured
```

## 7 - Verificando atualização dos pods

```
kubectl get pods -w
```

### Resultado:

```
NAME                             READY   STATUS    RESTARTS   AGE
my-deployment-6c797df6c6-2mts2   0/1     Running   0          2m22s
my-deployment-6c797df6c6-7m6f6   0/1     Running   0          2m22s
my-deployment-6c797df6c6-vpjct   0/1     Running   0          2m22s
my-deployment-86dbb8ff-mbjds     0/1     Running   0          25s
my-deployment-86dbb8ff-rs4wd     0/1     Running   0          25s
...
```

## 8 - Deletar deployment

```
kubectl delete deployment my-deployment
```

### Resultado:

```
deployment.apps "my-deployment" deleted
```

### Lista de deployment após delete

```
NAMESPACE     NAME      READY   UP-TO-DATE   AVAILABLE   AGE
kube-system   coredns   2/2     2            2           24h
```