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

# Network Policy [Network Policy](https://kubernetes.io/docs/concepts/services-networking/network-policies/), [Hand Dirty](https://kubernetes.io/docs/tasks/administer-cluster/declare-network-policy/)
>> it controls traffic flow for a pod 
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
##### Ingress and Egress
`- be careful of the three dash '-' from ipBlock, namespaceSelector and podSelector, that means match either of them. namespaceSelector is the same namespace with the its network policy. Eg: here networkpolicy and namespaceSelector is the same default namespace`
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

# Good Links
- [Overview of kubectl](https://kubernetes.io/docs/reference/kubectl/overview/)



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
