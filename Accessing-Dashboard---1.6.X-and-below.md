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

Once proxy server is started you should be able to access Dashboard from your browser at: `http://localhost:8001/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy/`.

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
kind: Service
metadata:
  creationTimestamp: 2017-09-11T08:00:46Z
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kube-system
  resourceVersion: "1300"
  selfLink: /api/v1/namespaces/kube-system/services/kubernetes-dashboard
  uid: 51392867-96c7-11e7-87e0-901b0e532516
spec:
  clusterIP: 10.103.169.125
  externalTrafficPolicy: Cluster
  ports:
  - port: 80
    protocol: TCP
    targetPort: 9090
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
kubernetes-dashboard   10.103.169.125   <nodes>       80:32703/TCP   1d
```

Dashboard has been exposed on a port `32703`. Now you can access it from your browser at: `http://<master-ip>:32703`. `master-ip` can be found by executing `kubectl cluster-info`. Usually it is either `127.0.0.1` or IP of your machine, assuming that you cluster is running directly on the machine, on which these commands are executed.

## API Server

In case Kubernetes API server is exposed and accessible from outside you can directly access dashboard at: `http(s)://<master-ip>:<apiserver-port>/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy/`.

## Ingress

Dashboard can be also exposed using Ingress resource. For more information check: https://kubernetes.io/docs/concepts/services-networking/ingress.