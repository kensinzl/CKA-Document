# Monitor, Log and Debug
[Debug Pods and ReplicationControllers](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/)
- Pod - Pending; cause should be insufficient CPU or Memory. [Pending Pod Example](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application-introspection/), **kubectl describe pod <pod-name>** to find the related information on the events.
- Pod - Waiting; casue should be the wrong image name which can not be pull, try manually docker image pull into local machine.

# Pod Generate
- kubectl run <pod_name> --image = <image_name> --namespace = <name_space> --labels = <label_name> --replicas = 1 --dry-run = client -o yaml
- yaml style: *NOT RECOMMEND*
[Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

# Persistence Volume
- [Emptydir](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir), multiple containers among pod can share the information in this volume, and if the pod is gone, then all volume is also gone.

# DaemonSet
>> A DaemonSet ensures that all (or some) Nodes run a copy of a Pod, depends on the nodes' taint and toleration.

>> As nodes are added to the cluster, Pods are added to them. 

>> As nodes are removed from the cluster, those Pods are garbage collected. 

>> Deleting a DaemonSet will clean up the Pods it created.

[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)

```sh
metadata:
  name: fluentd-elasticsearch
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
     name: fluentd-elasticsearch  
  template:
    metadata:
      labels:
       name: fluentd-elasticsearch
    spec:
      containers:
        .......
```
**spec.selector.matchLabels** define what pods' label DaemonSet would take care.

**spec.template.metadata.labels** pods' label, **spec.template** is pod template.

*So, the label value of **spec.selector.matchLabels** must be same to **spec.template.metadata.labels***

**metadata.labels** is just DaemonSet its label value.

# Deployment
```sh
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        ....
```
**spec.selector.matchLabels** is what pods' label deployment take care.

**spec.template.metadata.labels** is pods' label, **spec.template** is pod template.

*So, the label value of **spec.selector.matchLabels** must be same to **spec.template.metadata.labels***

**metadata.labels** is just Deployment itself label value.

# Assigning Pods to Nodes, [Assign Pod Node](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)
- NodeSelector: Specifies a map of node's key-value pairs. It can put deployment or pod yaml file.
[Assign Pod Node - Pod](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)
[Assign Pod Node - Deployment](https://kubernetes.io/docs/setup/production-environment/windows/user-guide-windows-containers/)
- NodeName: Typically match the node name. **Not Recommend**
- Taint: Allow node to repeal pods.  **key=value:effect:operator**, [taint-and-toleration](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)
>> Add taint for a node / remove a taint from a node

>> This means that no pod will be able to schedule onto node1 unless it has a matching toleration.

>> Taints and tolerations work together to ensure that pods are not scheduled onto inappropriate nodes.

>> Operator default value is 'Equal' which means match value and effect.
```sh
root@kubemaster:~/CKA# kubectl describe nodes kubemaster | grep -i Taints
Taints:             node-role.kubernetes.io/master:NoSchedule
```
```sh
kubectl taint nodes node1 key1=value1:NoSchedule
kubectl taint nodes node1 key1=value1:NoSchedule-
```

- Toleration: Allows but not required to the pods on the nodes with matching taint.

- Affinity: It is the property of pods which attract them a set of node.
[Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/), How to add the label for a node, *kubectl label nodes <node_name> mytest=awesome*
[Assign Pods Via Node Affinity](https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes-using-node-affinity/)
[Node Affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity)

|  | Node | Pod |
| :---: | :---: | :---: |
| Taint | ◯ | × |
| Toleration | × | ◯ |
| Affinity | ◯ | ◯ |
| NodeSelector | ◯ | ◯ |
| NodeName| ◯ | ◯ | 

# Secret, [Refer 1](https://feisky.gitbooks.io/kubernetes/content/concepts/secret.html), [Refer 2](https://medium.com/better-programming/how-to-use-kubernetes-secrets-for-storing-sensitive-config-data-f3c5e7d11c15)
Kubernetes Secrets let you store and manage sensitive information, such as passwords. Secrets can be mounted as data volumes or exposed as environment variables to be used by a container in a Pod.
- Opaque Secret, using base64 to decode, eg: *echo MWYyZDFlMmU2N2Rm | base64 --decode*
>> no matter **describe**, **-o yaml**, both show the encode style
>> only following two styles show the decode value
```sh
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  password: MWYyZDFlMmU2N2Rm
  username: YWRtaW4=
```

- Data Volumes Utilization
>> **cat /ect/foo/my-group/my-username** will get the value of key: username
>> the value is after base64 decode
```sh
spec:
  containers:
  - name: mypod
    image: redis
    volumeMounts:
    - name: foo
      mountPath: "/etc/foo"
      readOnly: true
  volumes:
  - name: foo
    secret:
      secretName: mysecret
      items:
      - key: username
        path: my-group/my-username
```

- Environment Variables
>> enter the pod, *echo $SECRET_USERNAME, echo $SECRET_PWD to show the value after base64 decode* 
```sh
  containers:
    - name: nginx
      image: nginx
      env:
        - name: SECRET_USERNAME
          valueFrom:
            secretKeyRef:
              name: mysecret
              key: username
        - name: SECRET_PWD
          valueFrom:
            secretKeyRef:
              name: mysecret
              key: password
```

# RBAC - role based access control [Refer1](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) [Refer2](https://www.jianshu.com/p/3ef44ddfbed3)
- How to create new user
```sh
1. openssl genrsa -out employee.key 2048
2. openssl req -new -key employee.key -out employee.csr -subj "/CN=employee"
3. openssl x509 -req -in employee.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out employee.crt -days 3650  # sign the employee to CA
4. kubectl config set-credentials employee --client-certificate=./employee.crt  --client-key=./employee.key  # add a new user
5. kubectl config view -o jsonpath='{.users[*].name}'   # user: employee is already set up
6. kubectl config set-context employee-context --cluster=kubernetes --user=employee  # add employee-context to the context using a specific username
7. kubectl config get-contexts   # display list of contexts, now should has the default and employee-context, * means current
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
          employee-context              kubernetes   employee           
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   
8. kubectl config current-context   # get current context
-- kubernetes-admin@kubernetes
9. kubectl config use-context employee-context   # switch the context to employee-context
10. kubectl get pods
Error from server (Forbidden): nodes is forbidden: User "employee" cannot list resource "nodes" in API group "" at the cluster scope -- because user: employee still have no role binding
11. kubectl --context=kubernetes-admin@kubernete get pods # although current context is not default still can use context=default to fetch
```
- How to let employee access the pod
```sh
kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: deployment-manager
rules:
- apiGroups: ["", "extensions", "apps"]
  resources: ["deployments", "replicasets", "pods"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"] # You can also use ["*"]
```
```sh
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: deployment-manager-binding
subjects:
- kind: User
  name: employee
  apiGroup: ""
roleRef:
  kind: Role
  name: deployment-manager
  apiGroup: ""
```

###### Now try *kubectl get pods* among employee user can work. but service we do not put on the resource, so *kubectl get service* still not work

>>root@kubemaster:~/CKA# kubectl get services
Error from server (Forbidden): services is forbidden: User "employee" cannot list resource "services" in API group "" in the namespace "default"

`This table describes namespace condition. `
- `NameSpace of role confines the role access which particular namespace.`
- `NameSpace of role binding confines the subject(role target) access which particular namespace which be same with role's namespace`
- `resourceNames of role/cluster role can confine's resource further. `
`Eg: there are red, blue and white pod in the dev namespace.`
`The following spnit just let you touch the red and white pod exclude blue one`

```sh
kind: Role
metadata:
  namespace: dev
  name: pod-role
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pod"]
  resourceNames: ["red", "white"]
  verbs: ["update", "get"]
```
|  | RoleBing`[DEV]` | ClusterRoleBing |
| ------ | ------ |------ |
| Role`[DEV]` | NameSpace have to same, `DEV` | Not Common Case |
| ClusterRole | `DEV` | All NameSpaces |

- `apiGroups value can use kubectl api-resources -o wide, Eg, pod is on the core API, deploy is on the apps`
   `that is why role for deploy need use apps apiGroup`
   `if you type wrong like apps -> app, then not work`
- `take care the role binding subject.kind namespace, see the mock example`
```sh
rules:
- apiGroups: ["extensions", "apps"]
  resources: ["deployments"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```

##### kubectl api-resources --namespaced=true `what resources need namespaces`
##### kubectl api-resources --namespaced=false `what resources do not need namespaces`


# Network Policy [Network Policy](https://kubernetes.io/docs/concepts/services-networking/network-policies/), [Hand Dirty](https://kubernetes.io/docs/tasks/administer-cluster/declare-network-policy/), [Good View](https://medium.com/@reuvenharrison/an-introduction-to-kubernetes-network-policies-for-security-people-ba92dd4c809d)
>> it controls traffic flow for a pod 
>> `"role=db" pods in the "default" namespace` from [Network Policy](https://kubernetes.io/docs/concepts/services-networking/network-policies/), will know metadata.namespace is the namespace of spec.podSelector pod
>> podSelector: any pod in the "default" namespace from metadata.namespace with the label "role=frontend"
>> namespaceSelector: any pod in a namespace with the label "project=myproject"

##### selects all pods but does not allow any ingress traffic to those pods. podSelector {} means all pods
```sh
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

##### allow all traffic to all pods. ingress {} means all traffic
```sh
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-all-ingress
spec:
  podSelector: {}
  ingress:
  - {}
  policyTypes:
  - Ingress
```
#####  selects all pods does not allow any egress traffic from those pods.
```sh
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-egress
spec:
  podSelector: {}
  policyTypes:
  - Egress
```

##### allow all traffic from all pods.
```sh
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-all-egress
spec:
  podSelector: {}
  egress:
  - {}
  policyTypes:
  - Egress
```
##### deny all ingress and all egress traffic
```sh
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```
##### multiple egress. the ingress's port belongs to the podSelector, but egress's pod belong to each rule's port.
```sh
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: internal-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      name: internal
  policyTypes:
  - Egress
  egress:
  - to:
    - podSelector:
        matchLabels:
          name: payroll
    ports:
    - protocol: TCP
      port: 8080
  - to:
    - podSelector:
        matchLabels:
          name: mysql
    ports:
    - protocol: TCP
      port: 3306
```


# Ingress and Egress
`- be careful of the three dash '-' from ipBlock, namespaceSelector and podSelector, that means match either of them. podSelector is the same namespace with the its network policy. Eg: here networkpolicy and podSelector is the same default namespace`
`- this example shows how to control the Ingress and Egress of one pod which label is (role=db)`
```sh
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector:
        matchLabels:
          project: myproject
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 6379
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - protocol: TCP
      port: 5978
```

`only one dash means both namespaceSelector and podSelector match to select the particular Pods`
```sh
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          user: alice
      podSelector:
        matchLabels:
          role: client
```
`match either of namespaceSelector of podSelector.Contains two elements in the from array, and allows connections from Pods in the local Namespace with the label role=client, or from any Pod in any namespace with the label user=alice.`
```sh
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          user: alice
    - podSelector:
        matchLabels:
          role: client
```

# DNS
>>> The pod is ephemeral, so we should use service to access the pod.
>>> We can use curl http://<pod_internal_ip>:<pod_port> from one pod.
>>> But remembering ip is tediour, so better directly use service name.
>>> We can use curl http://<service>:<service_port> from one pod.
>>> So DNS is to use predictable name to resolve the particular IP.
>>> K8S DNS now is coredns pods among kube-system namespaces.
```sh
$ kubectl get pods -o wide
NAME                        READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
dnsutils                    1/1     Running   0          10m   172.18.0.6   minikube   <none>           <none>
nginx-dns-5b85bb5dc-ssxpl   1/1     Running   0          13s   172.18.0.7   minikube   <none>           <none>
$ kubectl get svc -o wide
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE   SELECTOR
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP   15m   <none>
nginx-dns    ClusterIP   10.107.120.165   <none>        80/TCP    69s   run=nginx-dns
$ kubectl exec -it dnsutils -- curl http://172.18.0.7:80
$ kubectl exec -it dnsutils -- curl http://nginx-dns:80 or http://10.107.120.165:80
$ kubectl exec -it dnsutils -- bin/ash
/ # nslookup nginx-dns
Server:         10.96.0.10
Address:        10.96.0.10#53

Name:   nginx-dns.default.svc.cluster.local => <hostname>.<namespace>.<type>.<root>
Address: 10.107.120.165
```

### Maintain the NODE
#####   kubectl drain <node_name>
>>> 1. Drain all pods existing in this node, exluding the daemonset because daemonset have to exist in each node.
>>> 2. If pods not managed by the replicaset, the drained pod will be gone forever. So better to make to backup for the pod's yaml.
>>> 3. After draining, the node will become unschedule, that means the node be with the taint: unschedule.

#####   kubectl cordon <node_name>
>>> 1. Making node into unschedule.
>>> 2. Do not drain the existing pods in this node.

#####   kubectl uncordon <node_name>
>>> 1. Making the node back to normal to accept the new pods allocation.

`Eg1: The following describe Daemonsets pods can not be drained.`
```sh
controlplane $ kubectl drain node01
node/node01 cordoned
error: unable to drain node "node01", aborting command...

There are pending nodes to be drained:
 node01
error: cannot delete DaemonSet-managed Pods (use --ignore-daemonsets to ignore): kube-system/kube-flannel-ds-amd64-vkqxk, kube-system/kube-proxy-9hndl
controlplane $ kubectl get pods -n kube-system -o wide
NAME                                   READY   STATUS    RESTARTS   AGE     IP            NODE           
coredns-f9fd979d6-nzwxd                1/1     Running   0          5m7s    10.244.1.2    node03         
coredns-f9fd979d6-s55nd                1/1     Running   1          5m7s    10.244.3.3    node01         
etcd-controlplane                      1/1     Running   0          5m16s   172.17.0.10   controlplane   
kube-apiserver-controlplane            1/1     Running   0          5m16s   172.17.0.10   controlplane   
kube-controller-manager-controlplane   1/1     Running   0          5m16s   172.17.0.10   controlplane   
kube-flannel-ds-amd64-2rnsm            1/1     Running   0          5m7s    172.17.0.10   controlplane   
kube-flannel-ds-amd64-7c42p            1/1     Running   1          4m57s   172.17.0.53   node02         
kube-flannel-ds-amd64-qzsfz            1/1     Running   1          4m57s   172.17.0.20   node03         
kube-flannel-ds-amd64-vkqxk            1/1     Running   2          4m56s   172.17.0.40   node01         
kube-proxy-9hndl                       1/1     Running   2          4m56s   172.17.0.40   node01         
kube-proxy-hk72c                       1/1     Running   1          4m57s   172.17.0.53   node02         
kube-proxy-lfgxg                       1/1     Running   0          5m7s    172.17.0.10   controlplane   
kube-proxy-t66wx                       1/1     Running   1          4m57s   172.17.0.20   node03         
kube-scheduler-controlplane            1/1     Running   0          5m15s   172.17.0.10   controlplane   
```

`Eg2: After using --ignore-daemonsets, the daemonset is not ignored but others was drained.`
```sh
controlplane $ kubectl drain node01 --ignore-daemonsets
node/node01 already cordoned
WARNING: ignoring DaemonSet-managed Pods: kube-system/kube-flannel-ds-amd64-vkqxk, kube-system/kube-proxy-9hndl
evicting pod default/blue-746c87566d-2q5xn
evicting pod default/red-75f847bf79-hl72f
evicting pod kube-system/coredns-f9fd979d6-s55nd
pod/blue-746c87566d-2q5xn evicted
pod/red-75f847bf79-hl72f evicted
pod/coredns-f9fd979d6-s55nd evicted
node/node01 evicted
controlplane $ 
controlplane $ kubectl get pods -n kube-system -o wide
NAME                                   READY   STATUS    RESTARTS   AGE     IP            NODE        
coredns-f9fd979d6-nzwxd                1/1     Running   0          6m21s   10.244.1.2    node03      
coredns-f9fd979d6-s99x9                1/1     Running   0          19s     10.244.2.4    node02      
etcd-controlplane                      1/1     Running   0          6m30s   172.17.0.10   controlplane
kube-apiserver-controlplane            1/1     Running   0          6m30s   172.17.0.10   controlplane
kube-controller-manager-controlplane   1/1     Running   0          6m30s   172.17.0.10   controlplane
kube-flannel-ds-amd64-2rnsm            1/1     Running   0          6m21s   172.17.0.10   controlplane
kube-flannel-ds-amd64-7c42p            1/1     Running   1          6m11s   172.17.0.53   node02      
kube-flannel-ds-amd64-qzsfz            1/1     Running   1          6m11s   172.17.0.20   node03      
kube-flannel-ds-amd64-vkqxk            1/1     Running   2          6m10s   172.17.0.40   node01      
kube-proxy-9hndl                       1/1     Running   2          6m10s   172.17.0.40   node01      
kube-proxy-hk72c                       1/1     Running   1          6m11s   172.17.0.53   node02      
kube-proxy-lfgxg                       1/1     Running   0          6m21s   172.17.0.10   controlplane
kube-proxy-t66wx                       1/1     Running   1          6m11s   172.17.0.20   node03      
kube-scheduler-controlplane            1/1     Running   0          6m29s   172.17.0.10   controlplane
```

`Eg3: After draining, the node become unschedule and with unschedule taint and all pods is distributed into other nodes.`
```sh
controlplane $ kubectl get nodes
NAME           STATUS                     ROLES    AGE     VERSION
controlplane   Ready                      master   8m19s   v1.19.0
node01         Ready,SchedulingDisabled   <none>   7m48s   v1.19.0
node02         Ready                      <none>   7m49s   v1.19.0
node03         Ready                      <none>   7m49s   v1.19.0
controlplane $ 
controlplane $ kubectl get pods -o wide
NAME                    READY   STATUS    RESTARTS   AGE     IP           NODE     NOMINATED NODE   READINESS GATES
blue-746c87566d-gx6nn   1/1     Running   0          6m35s   10.244.1.3   node03   <none>           <none>
blue-746c87566d-rb4h7   1/1     Running   0          2m10s   10.244.1.4   node03   <none>           <none>
blue-746c87566d-tmm2r   1/1     Running   0          6m35s   10.244.2.3   node02   <none>           <none>
red-75f847bf79-8gzkl    1/1     Running   0          6m35s   10.244.2.2   node02   <none>           <none>
red-75f847bf79-fprhv    1/1     Running   0          2m9s    10.244.1.5   node03   <none>           <none>
controlplane $ 
controlplane $ kubectl describe nodes node01 | grep -i taint
Taints:             node.kubernetes.io/unschedulable:NoSchedule
```

`Eg4: Finish patches or updating, the node should uncordon to accept new pods allocation`
```sh
controlplane $ kubectl uncordon  node01
node/node01 uncordoned
controlplane $ kubectl get nodes
NAME           STATUS   ROLES    AGE   VERSION
controlplane   Ready    master   17m   v1.19.0
node01         Ready    <none>   16m   v1.19.0
node02         Ready    <none>   16m   v1.19.0
node03         Ready    <none>   16m   v1.19.0
```

`Eg5: The pods<hr-app> is not managed by Rs and if drained, it will be gone.`
```sh
controlplane $ kubectl get pods -o wide
NAME                    READY   STATUS    RESTARTS   AGE   IP           NODE     
blue-746c87566d-gx6nn   1/1     Running   0          21m   10.244.1.3   node03   
blue-746c87566d-rb4h7   1/1     Running   0          17m   10.244.1.4   node03   
blue-746c87566d-tmm2r   1/1     Running   0          21m   10.244.2.3   node02   
hr-app                  1/1     Running   0          29s   10.244.2.5   node02   
red-75f847bf79-8gzkl    1/1     Running   0          21m   10.244.2.2   node02   
red-75f847bf79-fprhv    1/1     Running   0          17m   10.244.1.5   node03   
controlplane $ 
controlplane $ kubectl get rs -o wide
NAME              DESIRED   CURRENT   READY   AGE   CONTAINERS   IMAGES         
blue-746c87566d   3         3         3       22m   nginx        nginx:alpine   
red-75f847bf79    2         2         2       22m   nginx        nginx:alpine   
controlplane $ kubectl drain node02
node/node02 cordoned
error: unable to drain node "node02", aborting command...

There are pending nodes to be drained:
 node02
cannot delete Pods not managed by ReplicationController, ReplicaSet, Job, DaemonSet or StatefulSet (use --force to override): default/hr-app
cannot delete DaemonSet-managed Pods (use --ignore-daemonsets to ignore): kube-system/kube-flannel-ds-amd64-7c42p, kube-system/kube-proxy-hk72c
controlplane $ kubectl drain node02 --force
node/node02 already cordoned
error: unable to drain node "node02", aborting command...

There are pending nodes to be drained:
 node02
error: cannot delete DaemonSet-managed Pods (use --ignore-daemonsets to ignore): kube-system/kube-flannel-ds-amd64-7c42p, kube-system/kube-proxy-hk72c
controlplane $ kubectl drain node02 --force --ignore-daemonsets
node/node02 already cordoned
WARNING: deleting Pods not managed by ReplicationController, ReplicaSet, Job, DaemonSet or StatefulSet: default/hr-app; ignoring DaemonSet-managed Pods: kube-system/kube-flannel-ds-amd64-7c42p, kube-system/kube-proxy-hk72c
evicting pod default/blue-746c87566d-tmm2r
evicting pod default/hr-app
evicting pod default/red-75f847bf79-8gzkl
evicting pod kube-system/coredns-f9fd979d6-s99x9
pod/red-75f847bf79-8gzkl evicted
pod/coredns-f9fd979d6-s99x9 evicted
pod/blue-746c87566d-tmm2r evicted
pod/hr-app evicted
node/node02 evicted
```

`Eg6: Cordon a node do not drain the existing pod just make it into unschedule`
```sh
controlplane $ kubectl get pods -o wide
NAME                    READY   STATUS    RESTARTS   AGE     IP           NODE     
blue-746c87566d-2zfxq   1/1     Running   0          7m53s   10.244.3.7   node01   
blue-746c87566d-gx6nn   1/1     Running   0          36m     10.244.1.3   node03   
blue-746c87566d-rb4h7   1/1     Running   0          31m     10.244.1.4   node03   
red-75f847bf79-fprhv    1/1     Running   0          31m     10.244.1.5   node03   
red-75f847bf79-hbhmt    1/1     Running   0          7m53s   10.244.3.6   node01   
controlplane $ kubectl cordon node03
node/node03 cordoned
controlplane $ kubectl get nodes
NAME           STATUS                     ROLES    AGE   VERSION
controlplane   Ready                      master   38m   v1.19.0
node01         Ready                      <none>   38m   v1.19.0
node02         Ready,SchedulingDisabled   <none>   38m   v1.19.0
node03         Ready,SchedulingDisabled   <none>   38m   v1.19.0
controlplane $ kubectl get pods -o wide
NAME                    READY   STATUS    RESTARTS   AGE     IP           NODE     
blue-746c87566d-2zfxq   1/1     Running   0          8m38s   10.244.3.7   node01   
blue-746c87566d-gx6nn   1/1     Running   0          36m     10.244.1.3   node03   
blue-746c87566d-rb4h7   1/1     Running   0          32m     10.244.1.4   node03   
red-75f847bf79-fprhv    1/1     Running   0          32m     10.244.1.5   node03   
red-75f847bf79-hbhmt    1/1     Running   0          8m38s   10.244.3.6   node01   
```

### Refer
[MiniKube Example - can pratice on Official Test](https://www.bogotobogo.com/DevOps/Docker/Docker_Kubernetes_DNS_with_Pods_Services.php)
[Official Test](https://kubernetes.io/docs/tutorials/hello-minikube/)
[Another Example](https://medium.com/kubernetes-tutorials/kubernetes-dns-for-services-and-pods-664804211501)
[Debugging DNS Resolution - Need See During Test](https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/)
[Hand Dirty](https://kodekloud.com/courses/certified-kubernetes-administrator-with-practice-tests-labs/lectures/12038776)

#### ETCD
##### The ETCD has the ambugious document, [Operating etcd clusters for Kubernetes](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/), which does not include restore stuff.
>>>> 1. ETCDCTL_API=3 etcdctl --endpoints=<IP> --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key snapshot save <SAVED_PATH>
`cacert, cert and key` are not on the document. You can use `ETCDCTL_API=3 etcdctl snapshot save --help` to get these three attributes, whose values are shown on the test.
>>>> 2. ETCDCTL_API=3 etcdctl snapshot status <SAVED_PATH> to check after saving ETCD. `Not necessarily to use certificate.`
>>>> 3. ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key snapshot restore /opt/snapshot-pre-boot.db. `Not necessarily to use certificate, better to use this on the test.`
>>>> 4. `ETCDCTL_API=3 etcdctl snapshot restore /opt/snapshot-pre-boot.db` is wrong, actually needs `--data-dir` which can get from `ETCDCTL_API=3 etcdctl snapshot restore --help if you do not use certificate`. `--data-dir` is the path where allocates the restored back-up.
>>>> 5. On the test, better to kubectl get nodes after restoring.

```sh
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key snapshot save /opt/snapshot-pre-boot.db

ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key snapshot restore /opt/snapshot-pre-boot.db
```

# Ingress
>>>> 1. Ingress occurs from service(NodePort, ClusterIP and LoadBalance)
NodePort has the port range limitation. ClusterIP only accept the internal IP. LoadBalance needs the charged third party.
Ingress is designed to fix weekness of LoadBalance.
[reference1](https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-ingress-guide-nginx-example.html)
[reference2](https://thenewstack.io/kubernetes-ingress-for-beginners/)
>>>> 2. In order for the Ingress to work, the cluster must have an ingress controller running. Unlike other types of controllers which run as part of the kube-controller-manager binary, Ingress controllers are not started automatically with a cluster. 
[Controllers List - Always Using Nginx](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/)
```sh
controlplane $ kubectl get pods --all-namespaces
NAMESPACE       NAME                                        READY   STATUS    RESTARTS   AGE
ingress-space   nginx-ingress-controller-697cfbd4d9-6j92h   1/1     Running   0          38m
```
>>>> 3. Ingress supports the following from [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) [MiniKube](https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/)
`1. Single host with multiple paths` 
`2. Multiple hosts with multiple paths` 
`3. No host, so the rule applies to all inbound HTTP traffic. ` 
`4. One path may relate to one service, if the service is NodePort, what configuring in the Ingress yaml file is the port of the service rather than NodePort. `
`5. defaultBackend is configured in an Ingress controller to service any requests that do not match a path in the spec. Which needs the default pod and service`
```sh
controlplane $ kubectl get pods --all-namespaces
NAMESPACE       NAME                                        READY   STATUS    RESTARTS   AGE
app-space       default-backend-5cf9bfb9d-b9mlw             1/1     Running   0          17m

controlplane $ kubectl get svc --all-namespaces
NAMESPACE       NAME                   TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
app-space       default-http-backend   ClusterIP   10.102.159.203   <none>        80/TCP                       12m
```
`6. Ingress modification no necessary to delete the current running one, just using "edit" the current running one like deployment.`
`7. Ingress's namespace should be the same with the service's namespace. But if the service is vital then need to put in a seprate namespace, then we should create a new Ingress. K8S can track the URL to find the related service`
[Last Example of hand dirty](https://kodekloud.com/courses/certified-kubernetes-administrator-with-practice-tests-labs/lectures/12038778)



# Good Links
- [Overview of kubectl](https://kubernetes.io/docs/reference/kubectl/overview/)
- [Update the node](https://v1-19.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)
- https://stackoverflow.com/questions/47107117/how-to-debug-when-kubernetes-nodes-are-in-not-ready-state
- [Updating Kubeadm Cluster](https://v1-19.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)
- [How to Create ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)



https://staight.github.io/2019/09/26/kubernetes%E4%B8%AD%E7%9A%84%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6-%E7%94%A8%E6%88%B7%E8%AE%A4%E8%AF%81/
# Dillinger

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Dillinger is a cloud-enabled, mobile-ready, offline-storage, AngularJS powered HTML5 Markdown editor.

  - Type some Markdown on the left
  - See HTML in the right
  - Magic

# New Features!

  - Import a HTML file and watch it magically convert to Markdown
  - Drag and drop images (requires your Dropbox account be linked)


You can also:
  - Import and save files from GitHub, Dropbox, Google Drive and One Drive
  - Drag and drop markdown and HTML files into Dillinger
  - Export documents as Markdown, HTML and PDF

Markdown is a lightweight markup language based on the formatting conventions that people naturally use in email.  As [John Gruber] writes on the [Markdown site][df1]

> The overriding design goal for Markdown's
> formatting syntax is to make it as readable
> as possible. The idea is that a
> Markdown-formatted document should be
> publishable as-is, as plain text, without
> looking like it's been marked up with tags
> or formatting instructions.

This text you see here is *actually* written in Markdown! To get a feel for Markdown's syntax, type some text into the left window and watch the results in the right.

### Tech

Dillinger uses a number of open source projects to work properly:

* [AngularJS] - HTML enhanced for web apps!
* [Ace Editor] - awesome web-based text editor
* [markdown-it] - Markdown parser done right. Fast and easy to extend.
* [Twitter Bootstrap] - great UI boilerplate for modern web apps
* [node.js] - evented I/O for the backend
* [Express] - fast node.js network app framework [@tjholowaychuk]
* [Gulp] - the streaming build system
* [Breakdance](https://breakdance.github.io/breakdance/) - HTML to Markdown converter
* [jQuery] - duh

And of course Dillinger itself is open source with a [public repository][dill]
 on GitHub.

### Installation

Dillinger requires [Node.js](https://nodejs.org/) v4+ to run.

Install the dependencies and devDependencies and start the server.

```sh
$ cd dillinger
$ npm install -d
$ node app
```

For production environments...

```sh
$ npm install --production
$ NODE_ENV=production node app
```

### Plugins

Dillinger is currently extended with the following plugins. Instructions on how to use them in your own application are linked below.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |


### Development

Want to contribute? Great!

Dillinger uses Gulp + Webpack for fast developing.
Make a change in your file and instantaneously see your updates!

Open your favorite Terminal and run these commands.

First Tab:
```sh
$ node app
```

Second Tab:
```sh
$ gulp watch
```

(optional) Third:
```sh
$ karma test
```
#### Building for source
For production release:
```sh
$ gulp build --prod
```
Generating pre-built zip archives for distribution:
```sh
$ gulp build dist --prod
```
### Docker
Dillinger is very easy to install and deploy in a Docker container.

By default, the Docker will expose port 8080, so change this within the Dockerfile if necessary. When ready, simply use the Dockerfile to build the image.

```sh
cd dillinger
docker build -t joemccann/dillinger:${package.json.version} .
```
This will create the dillinger image and pull in the necessary dependencies. Be sure to swap out `${package.json.version}` with the actual version of Dillinger.

Once done, run the Docker image and map the port to whatever you wish on your host. In this example, we simply map port 8000 of the host to port 8080 of the Docker (or whatever port was exposed in the Dockerfile):

```sh
docker run -d -p 8000:8080 --restart="always" <youruser>/dillinger:${package.json.version}
```

Verify the deployment by navigating to your server address in your preferred browser.

```sh
127.0.0.1:8000
```

#### Kubernetes + Google Cloud

See [KUBERNETES.md](https://github.com/joemccann/dillinger/blob/master/KUBERNETES.md)


### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT


**Free Software, Hell Yeah!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>
