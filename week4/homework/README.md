# 1. Napisz czy wybrałeś minikube czy K8s i pokaż nam odpowiedzi z komend, które wykonałeś

> Używam K8s, dostępnego w ramach środowiska Docker for Windows

`kubectl cluser-info`

```
$ kubectl cluster-info
Kubernetes master is running at https://kubernetes.docker.internal:6443
KubeDNS is running at https://kubernetes.docker.internal:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

---

`kubectl get pods –-all-namespaces`

```
$ kubectl get pods --all-namespaces
NAMESPACE     NAME                                     READY   STATUS    RESTARTS   AGE
default       nginx-ng-1-798bb796f8-747xx              1/1     Running   0          47h
docker        compose-6c67d745f6-t486x                 1/1     Running   2          2d3h
docker        compose-api-57ff65b8c7-xbx79             1/1     Running   2          2d3h
kube-system   coredns-6dcc67dcbc-nkxkw                 1/1     Running   2          2d3h
kube-system   coredns-6dcc67dcbc-xt2qs                 1/1     Running   2          2d3h
kube-system   etcd-docker-desktop                      1/1     Running   2          2d3h
kube-system   kube-apiserver-docker-desktop            1/1     Running   2          2d3h
kube-system   kube-controller-manager-docker-desktop   1/1     Running   2          2d3h
kube-system   kube-proxy-kffc4                         1/1     Running   2          2d3h
kube-system   kube-scheduler-docker-desktop            1/1     Running   2          2d3h
```

---

`kubectl get nodes`

```
$ kubectl get nodes
NAME             STATUS   ROLES    AGE    VERSION
docker-desktop   Ready    master   2d3h   v1.14.7
```

# 2. Napisz jaką chmurę wybrałeś i jak wyglądał Twój proces postawienia pełnego klastra (użyłeś portalu, CLI, a może Cloud Formation/ARM)

> Wybrałem Azure i użyłem CLI

1. Stworzyłem `resource group CloudHomeworkRG`

`az group create --name CloudHomeworkRG --location westeurope`

2. Stworzyłem klaster Kubernetesa `CloudHomeworkAKSCluster` z liczbą nodów 4 z włączonym monitoringiem

`az aks create -g CloudHomeworkRG -n CloudHomeworkAKSCluster -c 4 --enable-addons monitoring`

3. Ustawiłem kontekst dla stworzonego klastra AKS w `kubectl`

`az aks get-credentials -g CloudHomeworkRG -n CloudHomeworkAKSCluster`

```
kubectl config current-context
CloudHomeworkAKSCluster
```

# 3. Pokaż jakie polecenia kubectl wykonałeś i jaki był ich rezultat

`kubectl get nodes` - pobierz listę nodów.

```
kubectl get nodes
NAME                                STATUS   ROLES   AGE     VERSION
aks-nodepool1-26673989-vmss000000   Ready    agent   7m57s   v1.13.12
aks-nodepool1-26673989-vmss000001   Ready    agent   8m5s    v1.13.12
aks-nodepool1-26673989-vmss000002   Ready    agent   7m54s   v1.13.12
aks-nodepool1-26673989-vmss000003   Ready    agent   8m1s    v1.13.12
```

`kubectl get pods --all-namespaces` - pobierz listę podów ze wszystkich przestrzeni nazw

```
kubectl get pods --all-namespaces
NAMESPACE     NAME                                    READY   STATUS    RESTARTS   AGE
kube-system   coredns-866fc6b6c8-5v7zq                1/1     Running   0          15m
kube-system   coredns-866fc6b6c8-zjn7w                1/1     Running   0          10m
kube-system   coredns-autoscaler-657d77ffbf-kfkr8     1/1     Running   0          14m
kube-system   kube-proxy-5t7nj                        1/1     Running   0          11m
kube-system   kube-proxy-mrnpj                        1/1     Running   0          11m
kube-system   kube-proxy-nkdtb                        1/1     Running   0          11m
kube-system   kube-proxy-vlpzx                        1/1     Running   0          11m
kube-system   kubernetes-dashboard-6f697bd9f5-v4xl9   1/1     Running   2          15m
kube-system   metrics-server-58699455bc-bhsrf         1/1     Running   0          15m
kube-system   omsagent-6crcm                          1/1     Running   1          11m
kube-system   omsagent-frvjk                          1/1     Running   1          11m
kube-system   omsagent-rs-5869b6ffc4-njr2b            1/1     Running   1          15m
kube-system   omsagent-wxk2l                          1/1     Running   1          11m
kube-system   omsagent-xl8rw                          1/1     Running   0          11m
kube-system   tunnelfront-5b44ffd89-dxhxx             1/1     Running   0          14m
```

`kubectl describe node/aks-nodepool1-26673989-vmss000000` - szczegółowy opis jednego noda

> Fragment odpowiedzi

```
kubectl describe nodes aks-nodepool1-26673989-vmss000000
Name:               aks-nodepool1-26673989-vmss000000
Roles:              agent
Labels:             agentpool=nodepool1
                    beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/instance-type=Standard_DS2_v2
                    beta.kubernetes.io/os=linux
                    failure-domain.beta.kubernetes.io/region=westeurope
                    failure-domain.beta.kubernetes.io/zone=0
                    kubernetes.azure.com/cluster=MC_CloudHomeworkRG_CloudHomeworkAKSCluster_westeurope
                    kubernetes.azure.com/role=agent
                    kubernetes.io/hostname=aks-nodepool1-26673989-vmss000000
                    kubernetes.io/role=agent
                    node-role.kubernetes.io/agent=
                    storageprofile=managed
                    storagetier=Premium_LRS
Annotations:        node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Sat, 02 Nov 2019 15:07:34 +0100
Taints:             <none>
Unschedulable:      false
Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----                 ------  -----------------                 ------------------                ------                       -------
  NetworkUnavailable   False   Sat, 02 Nov 2019 15:09:15 +0100   Sat, 02 Nov 2019 15:09:15 +0100   RouteCreated                 RouteController created a route
  MemoryPressure       False   Sat, 02 Nov 2019 15:21:49 +0100   Sat, 02 Nov 2019 15:07:34 +0100   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         False   Sat, 02 Nov 2019 15:21:49 +0100   Sat, 02 Nov 2019 15:07:34 +0100   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure          False   Sat, 02 Nov 2019 15:21:49 +0100   Sat, 02 Nov 2019 15:07:34 +0100   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready                True    Sat, 02 Nov 2019 15:21:49 +0100   Sat, 02 Nov 2019 15:07:34 +0100   KubeletReady                 kubelet is posting ready status. AppArmor enabled
Addresses:
  Hostname:    aks-nodepool1-26673989-vmss000000
  InternalIP:  10.240.0.4
Capacity:
 attachable-volumes-azure-disk:  8
 cpu:                            2
 ephemeral-storage:              101584140Ki
 hugepages-1Gi:                  0
 hugepages-2Mi:                  0
 memory:                         7113636Ki
 pods:                           110
Allocatable:
 attachable-volumes-azure-disk:  8
 cpu:                            1900m
 ephemeral-storage:              93619943269
 hugepages-1Gi:                  0
 hugepages-2Mi:                  0
 memory:                         4668324Ki
 pods:                           110
System Info:
 Kernel Version:             4.15.0-1060-azure
 OS Image:                   Ubuntu 16.04.6 LTS
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://3.0.7
 Kubelet Version:            v1.13.12
 Kube-Proxy Version:         v1.13.12
```

`kubectl get componentstatuses` - pokaż status schedulera, controllera, etcd

```
 kubectl get componentstatuses
NAME                 STATUS      MESSAGE                                                                                     ERROR
scheduler            Unhealthy   Get http://127.0.0.1:10251/healthz: dial tcp 127.0.0.1:10251: connect: connection refused
controller-manager   Unhealthy   Get http://127.0.0.1:10252/healthz: dial tcp 127.0.0.1:10252: connect: connection refused
etcd-0               Healthy     {"health": "true"}
```

`kubectl api-versions` - pokaż listę użytych wersji API

```
kubectl api-versions
admissionregistration.k8s.io/v1alpha1
admissionregistration.k8s.io/v1beta1
apiextensions.k8s.io/v1beta1
apiregistration.k8s.io/v1
apiregistration.k8s.io/v1beta1
apps/v1
apps/v1beta1
apps/v1beta2
authentication.k8s.io/v1
authentication.k8s.io/v1beta1
authorization.k8s.io/v1
authorization.k8s.io/v1beta1
autoscaling/v1
autoscaling/v2beta1
autoscaling/v2beta2
azmon.container.insights/v1
batch/v1
batch/v1beta1
certificates.k8s.io/v1beta1
coordination.k8s.io/v1beta1
events.k8s.io/v1beta1
extensions/v1beta1
metrics.k8s.io/v1beta1
networking.k8s.io/v1
policy/v1beta1
rbac.authorization.k8s.io/v1
rbac.authorization.k8s.io/v1beta1
scheduling.k8s.io/v1beta1
storage.k8s.io/v1
storage.k8s.io/v1beta1
v1
```
