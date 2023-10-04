# Pod

## 1 - Criar arquivo my-first-pod.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: "my-first-pod"
  namespace: default
  labels:
    app: "my-first-pod"
spec:
  containers:
  - name: my-first-pod
    image: "nginx:latest"
    resources:
      limits:
        cpu: 200m
        memory: 500Mi
      requests:
        cpu: 100m
        memory: 200Mi
```

## 2 - Aplicar pod

```
kubectl apply -f .\my-firts-pod.yaml
```

### Resultado:
```
pod/my-first-pod created
```

## 3 - Listar pods do cluster

```
kubectl get pods -A
```

### Resultado: 

```
NAMESPACE     NAME                                     READY   STATUS    RESTARTS        AGE
default       my-first-pod                             1/1     Running   0               5m21s
kube-system   coredns-5d78c9869d-jm4sv                 1/1     Running   1 (7m50s ago)   23h
kube-system   coredns-5d78c9869d-nlmpq                 1/1     Running   1 (7m50s ago)   23h
kube-system   etcd-docker-desktop                      1/1     Running   1 (7m50s ago)   23h
kube-system   kube-apiserver-docker-desktop            1/1     Running   1 (7m50s ago)   23h
kube-system   kube-controller-manager-docker-desktop   1/1     Running   8 (7m50s ago)   23h
kube-system   kube-proxy-5kstf                         1/1     Running   1 (7m50s ago)   23h
kube-system   kube-scheduler-docker-desktop            1/1     Running   5 (7m50s ago)   23h
kube-system   storage-provisioner                      1/1     Running   5 (7m22s ago)   23h
kube-system   vpnkit-controller                        1/1     Running   1 (7m50s ago)   23h
```

## 4 - Inspecionando o pod

```
kubectl describe pod my-first-pod
```

### Resultado:

```
Name:             my-first-pod
Namespace:        default
Priority:         0
Service Account:  default
Node:             docker-desktop/192.168.65.3
Start Time:       Wed, 04 Oct 2023 17:14:07 -0300
Labels:           app=my-first-pod
Annotations:      <none>
Status:           Running
IP:               10.1.0.72
IPs:
  IP:  10.1.0.72
Containers:
  my-first-pod:
    Container ID:   docker://9068e22ee882a7bdefd3dccec2832c209b409164b5af78271dcefda6dd0453a1
    Image:          nginx:latest
    Image ID:       docker-pullable://nginx@sha256:32da30332506740a2f7c34d5dc70467b7f14ec67d912703568daff790ab3f755
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Wed, 04 Oct 2023 17:14:10 -0300
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     200m
      memory:  500Mi
    Requests:
      cpu:        100m
      memory:     200Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-lrz47 (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  kube-api-access-lrz47:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  58s   default-scheduler  Successfully assigned default/my-first-pod to docker-desktop
  Normal  Pulling    58s   kubelet            Pulling image "nginx:latest"
  Normal  Pulled     55s   kubelet            Successfully pulled image "nginx:latest" in 2.668603121s (2.6686136s including waiting)
  Normal  Created    55s   kubelet            Created container my-first-pod
  Normal  Started    55s   kubelet            Started container my-first-pod
```

## 5 - Deletar pod

```
kubectl delete pod my-first-pod
```

### Resultado:

```
pod "my-first-pod" deleted
```

### Lista de pods apos delete

```
NAMESPACE     NAME                                     READY   STATUS    RESTARTS      AGE
kube-system   coredns-5d78c9869d-jm4sv                 1/1     Running   1 (11m ago)   23h
kube-system   coredns-5d78c9869d-nlmpq                 1/1     Running   1 (11m ago)   23h
kube-system   etcd-docker-desktop                      1/1     Running   1 (11m ago)   23h
kube-system   kube-apiserver-docker-desktop            1/1     Running   1 (11m ago)   23h
kube-system   kube-controller-manager-docker-desktop   1/1     Running   8 (11m ago)   23h
kube-system   kube-proxy-5kstf                         1/1     Running   1 (11m ago)   23h
kube-system   kube-scheduler-docker-desktop            1/1     Running   5 (11m ago)   23h
kube-system   storage-provisioner                      1/1     Running   5 (11m ago)   23h
kube-system   vpnkit-controller                        1/1     Running   1 (11m ago)   23h
```