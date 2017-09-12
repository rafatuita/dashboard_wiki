Development releases are build by our countinous integration on every successful master build. They are pushed to [`kubernetesdashboarddev/kubernetes-dashboard-$ARCH`](https://hub.docker.com/r/kubernetesdashboarddev)
repositories. Each build produces one image for each architecture. The images are then tagged
with SHA of the commit they were built at and `head` tag is updated to reference the newest one. It means that you can use them to test all the latest features and improvements, but they are not as stable as official releases. Following sections describe installation and discovery of development releases.

## Installation

In most of the use cases you need to execute the following command to deploy latest development release:

```bash
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/kubernetes-dashboard-head.yaml
```

However, if you are not using RBAC and/or your cluster is running on ARM you might be interested in other deployments:

```
kubernetes-dashboard-arm-head-no-rbac.yaml
kubernetes-dashboard-arm-head.yaml
kubernetes-dashboard-head-no-rbac.yaml
```
To install them you need to replace `<image>` in following command:

```bash
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/<image>
```

## Update to latest release

Once installed, the deployment is not automatically updated. In order to update it you need to delete the deployment's pod and wait for it to be recreated. After recreation, it should use the latest image.

Start by listing the pods in the `kube-system` namespace:
```sh
$ kubectl --namespace=kube-system get pods
NAME                                        READY     STATUS    RESTARTS   AGE
...
kubernetes-dashboard-1230846811-b95sv       1/1       Running   0          5m
```

And then delete the Dashboard's pods in the `kube-system` namespace:
```sh
$ kubectl --namespace=kube-system delete pods kubernetes-dashboard-1230846811-b95sv
pod "kubernetes-dashboard-1230846811-b95sv" deleted
```