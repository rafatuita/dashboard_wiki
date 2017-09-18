**IMPORTANT:** HTTPS endpoints are only available if you used [Recommended Setup]() to deploy Dashboard.

## `kubectl proxy`

`kubectl proxy` creates proxy server between your machine and Kubernetes API server. By default it is only accessible locally (from the machine that started it).

First let's check if kubectl is properly configured and has access to the cluster. In case of error follow [this guide](https://kubernetes.io/docs/tasks/tools/install-kubectl/) to install and set up `kubectl`.
```sh
$ kubectl cluster-info
# Example output
Kubernetes master is running at https://192.168.30.148:6443
Heapster is running at https://192.168.30.148:6443/api/v1/namespaces/kube-system/services/heapster/proxy
KubeDNS is running at https://192.168.30.148:6443/api/v1/namespaces/kube-system/services/kube-dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

Start local proxy server.
```sh
$ kubectl proxy
Starting to serve on 127.0.0.1:8001
```

Once proxy server is started you should be able to access Dashboard from your browser.

To access HTTPS endpoint of dashboard go to: `http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:https/proxy`


To access HTTP endpoint of dashboard go to: `http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard:http/proxy`

**NOTE:** Dashboard should not be exposed publicly using `kubectl proxy` command as it only allows HTTP connection. For domains other than `localhost` and `127.0.0.1` it will not be possible to sign in. Nothing will happen after clicking `Sign in` button on login page.

## NodePort

This way of accessing Dashboard is only recommended for development environments in a single node setup. 

Edit `kubernetes-dashboard` service.
```sh
$ kubectl -n kube-system edit service kubernetes-dashboard
```

You should see `yaml` representation of the service. Change `type: ClusterIP` to `type: NodePort` and save file. If it's already changed go to next step.
```yaml
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: v1
...
  name: kubernetes-dashboard
  namespace: kube-system
  resourceVersion: "343478"
  selfLink: /api/v1/namespaces/kube-system/services/kubernetes-dashboard-head
  uid: 8e48f478-993d-11e7-87e0-901b0e532516
spec:
  clusterIP: 10.100.124.90
  externalTrafficPolicy: Cluster
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: 9090
  - name: https
    port: 443
    protocol: TCP
    targetPort: 8443
  selector:
    k8s-app: kubernetes-dashboard
  sessionAffinity: None
  type: ClusterIP
status:
  loadBalancer: {}
```

Next we need to check port on which Dashboard was exposed.
```sh
$ kubectl -n kube-system get service kubernetes-dashboard
NAME                   CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes-dashboard   10.100.124.90   <nodes>       80:30784/TCP,443:31707/TCP   21h
```

Dashboard has been exposed on ports `30784 (HTTP)` and `31707 (HTTPS)`. Now you can access it from your browser at: `http://<master-ip>:30784` or `https://<master-ip>:31707`. `master-ip` can be found by executing `kubectl cluster-info`. Usually it is either `127.0.0.1` or IP of your machine, assuming that your cluster is running directly on the machine, on which these commands are executed.

In case you are trying to expose Dashboard using NodePort on a multi-node cluster, then you have to find out IP of the node on which Dashboard is running to access it. Instead of accessing `http(s)://<master-ip>:<nodePort>` you should access `http(s)://<node-ip>:<nodePort>`.

## API Server

In case Kubernetes API server is exposed and accessible from outside you can directly access dashboard in two ways.

### Dashboard HTTP endpoint
`https://<master-ip>:<apiserver-port>/api/v1/namespaces/kube-system/services/kubernetes-dashboard:http/proxy`

### Dashboard HTTPS endpoint
`https://<master-ip>:<apiserver-port>/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:https/proxy`

## Ingress

Dashboard can be also exposed using Ingress resource. For more information check: https://kubernetes.io/docs/concepts/services-networking/ingress.