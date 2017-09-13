Once Dashboard is installed on your cluster it can be accessed in a few different ways. Note that this document does not describe all possible ways of accessing cluster applications. In case of any error while trying to access Dashboard, please first read our [FAQ]() and check [closed issues](https://github.com/kubernetes/dashboard/issues?q=is%3Aissue+is%3Aclosed). In most cases errors are caused by cluster configuration issues.

## Kubectl proxy

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

Start local proxy server:
```sh
$ kubectl proxy
Starting to serve on 127.0.0.1:8001
```

Once proxy server is started you should be able to access Dashboard from your browser at: `http://localhost:8001/ui`.

## NodePort

This way of accessing Dashboard is only recommended for development environments in a single node setup. 

## API Server

## Ingress

https://kubernetes.io/docs/concepts/services-networking/ingress/