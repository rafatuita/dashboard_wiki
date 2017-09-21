In case you did not find any answer here and in [closed issues](https://github.com/kubernetes/dashboard/issues?q=is%3Aissue+is%3Aclosed), [create new issue](https://github.com/kubernetes/dashboard/issues/new).

### I cannot see any graphs in Dashboard, how to enable them?

Make sure, that Heapster is up and running and Dashboard was able to connect with it. First you have to verify Heapster state with Dashboard or `kubectl` command. Then you should check Dashboard logs and look for `metric` and `Heapster` keywords. You can find more informations about Dashboard's Integrations [here](https://github.com/kubernetes/dashboard/wiki/Integrations).

### During development I receive a lot of strange errors in the browser's console. What may be wrong?

You probably need to update your npm dependencies. Run following commands from Dashboard's root directory:

```sh
$ rm -rf node_modules bower_components
$ npm i
```

### Why my `Go is not in the path`?

Running into an error like that probably means, that you need to rerun following command:

```sh
$ export PATH=$PATH:/usr/local/go/bin
```

### I receive `linux mounts: Path /var/lib/kubelet is mounted on / but it is not a shared mount` error. What to do?

Try to run:

```sh
$ sudo mount --bind /var/lib/kubelet /var/lib/kubelet
$ sudo mount --make-shared /var/lib/kubelet
```

You can find more information [here](https://github.com/kubernetes/kubernetes/issues/4869#issuecomment-193640483).

### I am seeing 404 errors when trying to access Dashbord. Dashboard resources can not be loaded.
```
GET https://<IP>/api/v1/namespaces/kube-system/services/kubernetes-dashboard/static/vendor.9aa0b786.css 
proxy:1 GET https://<IP>/api/v1/namespaces/kube-system/services/kubernetes-dashboard/static/app.8ebf2901.css 
proxy:5 GET https://<IP>/api/v1/namespaces/kube-system/services/kubernetes-dashboard/api/appConfig.json 
proxy:5 GET https://<IP>/api/v1/namespaces/kube-system/services/kubernetes-dashboard/static/app.68d2caa2.js 
proxy:5 GET https://<IP>/api/v1/namespaces/kube-system/services/kubernetes-dashboard/static/vendor.840e639c.js 
proxy:5 GET https://<IP>/api/v1/namespaces/kube-system/services/kubernetes-dashboard/api/appConfig.json 
proxy:5 GET https://<IP>/api/v1/namespaces/kube-system/services/kubernetes-dashboard/static/app.68d2caa2.js
```

**IMPORTANT:** There is a [known issue](https://github.com/kubernetes/kubernetes/issues/52729) related to Kubernetes 1.7.6 where `/ui` redirect does not work. Try to add trailing slash at the end of `/ui` redirect url: `http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/`

If this does not help then this means there is a problem with your cluster or you are trying to access Dashboard in a wrong way. Usually this happens when you try to expose Dashboard using `kubectl proxy` in a wrong way (i.e. missing permissions).

You can quickly check if accessing
`http://localhost:8001/api/v1/proxy/namespaces/kube-system/services/kubernetes-dashboard/` instead of `http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy` will work for you.

Other way of checking if your issue is related to Dashboard is to expose and access it using **NodePort** method described in our [Accessing Dashboard](https://github.com/kubernetes/dashboard/wiki/Accessing-dashboard) guide. This will allow you to access Dashboard directly without any proxy involved. 

If any of described methods will work then this means it is **not** a Dashboard issue and you should seek for help on [core](https://github.com/kubernetes/kubernetes) repository or better yet read [Kubernetes Documentation](https://kubernetes.io/docs/tasks/) first to understand how it works.