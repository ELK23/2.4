# 2.4

## limited-role.yaml

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-read-role
rules:
- apiGroups: [""]
  resources: ["pods", "pods/log"]
  verbs: ["get", "list"]
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["describe"]
```

## limited-rolebinding.yaml

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: limited-user-binding
  namespace: default
subjects:
- kind: User
  name: limited-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-read-role
  apiGroup: rbac.authorization.k8s.io
```

```
root@kubernetes:/home/veer/kubernetes/24# kubectl --kubeconfig=limited-user.kubeconfig get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          5m1s
root@kubernetes:/home/veer/kubernetes/24# kubectl --kubeconfig=limited-user.kubeconfig describe pod nginx
Name:             nginx
Namespace:        default
Priority:         0
Service Account:  default
Node:             kubernetes/10.3.33.22
Start Time:       Fri, 13 Jun 2025 08:50:32 +0300
Labels:           run=nginx
Annotations:      cni.projectcalico.org/containerID: 3a2b9c116c05e7c1a56eee9336d2743c9014df8d4d3531345111812576a60455
                  cni.projectcalico.org/podIP: 10.1.192.119/32
                  cni.projectcalico.org/podIPs: 10.1.192.119/32
Status:           Running
IP:               10.1.192.119
IPs:
  IP:  10.1.192.119
Containers:
  nginx:
    Container ID:   containerd://0fe91c42bc568489afc55eb1c8ad7ec6168abaeb9dc2bc84eb58c07e4c1064c6
    Image:          nginx
    Image ID:       docker.io/library/nginx@sha256:6784fb0834aa7dbbe12e3d7471e69c290df3e6ba810dc38b34ae33d3c1c05f7d
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Fri, 13 Jun 2025 08:50:38 +0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-x2jsr (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-x2jsr:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  5m5s  default-scheduler  Successfully assigned default/nginx to kubernetes
  Normal  Pulling    5m6s  kubelet            Pulling image "nginx"
  Normal  Pulled     5m    kubelet            Successfully pulled image "nginx" in 5.198s (5.198s including waiting). Image size: 72406859 bytes.
  Normal  Created    5m    kubelet            Created container: nginx
  Normal  Started    5m    kubelet            Started container nginx
root@kubernetes:/home/veer/kubernetes/24#
```
