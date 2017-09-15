## Development release

Besides official releases, there are also development releases, that are pushed after every successful master build. It is not advised to use them on production environment as they are less stable than the official ones. Following sections describe installation and discovery of development releases.

### Installation

In most of the use cases you need to execute the following command to deploy latest development release:

```bash
$ kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/kubernetes-dashboard-head.yaml
```

You can also deploy dashboard on ARM-based clusters:

```
$ kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/kubernetes-dashboard-arm-head.yaml
```

### Update

Once installed, the deployment is not automatically updated. In order to update it you need to delete the deployment's pods and wait for it to be recreated. After recreation, it should use the latest image.

Delete all Dashboard pods (assuming that Dashboard is deployed in `kube-system` namespace):
```sh
$ kubectl -n kube-system delete $(kubectl -n kube-system get pod -o name | grep dashboard)
pod "kubernetes-dashboard-3313488171-7706x" deleted
pod "kubernetes-dashboard-3313488171-ddkqd" deleted
pod "kubernetes-dashboard-3313488171-dpf9t" deleted
pod "kubernetes-dashboard-3313488171-jdz1n" deleted
pod "kubernetes-dashboard-3313488171-sxc9n" deleted
```