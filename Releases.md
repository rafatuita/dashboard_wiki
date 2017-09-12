## Official releases

Official releases are provided after significant improvements are done in the Dashboard. You can find them on our [releases](https://github.com/kubernetes/dashboard/releases) page. It is strongly advised to always use the latest version.

### Releasing new version

So you want to release a new version of Dashboard? Great, you just need to follow the steps below.

1. Test everything twice on Docker image and `gulp serve:prod`.
2. Send a pull request that increases version numbers in all files. Follow versioning guidelines. Files to keep in sync are listed below:
   - `bower.json`
   - `package.json`
   - `build/conf.js`
   - YAML files from `src/deploy`
3. Get the pull request reviewed and merged.
4. Create a git [release tag](https://github.com/kubernetes/dashboard/releases/) for the merged pull request. Release description should include a changelog.
5. Build and push production images to container registry. Use `gulp push-to-gcr:release`.
6. Update add-ons on the Kubernetes core repository. Dashboard add-on directory is [here](https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/dashboard). If the update is minor, all that needs to be done is to change image version number in the main controller config file (`dashboard-controller.yaml`), and other configs, as described in the header of the config. If the release is major, this needs coordination with Kubernetes core team and possibly alignment with the schedule of the core.
7. Update addon config in the [minikube](https://github.com/kubernetes/minikube/tree/master/deploy/addons) repo.

### Versioning guidelines

Kubernetes Dashboard versioning follows [semver](http://semver.org/) in spirit. This means
that is uses `vMAJOR.MINOR.PATCH` version numbers, but uses UX and consumer-centric approach for
incrementing version numbers.

1. Increment MAJOR when there are breaking changes that affect user's workflows or the UX gets
   major redesign.
1. Increment MINOR when new functionality is added or there are minor UX changes.
1. Increment PATCH in case of bug fixes and minor new features.

Versions `0.X.Y` are reserved for initial development and may not strictly follow the guidelines.

## Development releases

Besides official releases Dashboard's continuous integration provides development releases on every successful master build. They are pushed to [`kubernetesdashboarddev/kubernetes-dashboard-$ARCH`](https://hub.docker.com/r/kubernetesdashboarddev)
repositories. Each build produces one image for each architecture. The images are tagged
with SHA of the commit they were built at and `head` tag is updated to reference the newest one. It means that you can use them to test all the latest features and improvements, but they are not as stable as official releases. Following sections describe installation and discovery of development releases.

### Installing new release

In most of the use cases you need to execute the following command to deploy latest development release:

```sh
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/kubernetes-dashboard-head.yaml
```

However, if you are not using RBAC and/or your cluster is running on ARM you might be interested in other deployments:

```
kubernetes-dashboard-arm-head-no-rbac.yaml
kubernetes-dashboard-arm-head.yaml
kubernetes-dashboard-head-no-rbac.yaml
```
To install them you need to replace `<image>` in following command:

```sh
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/<image>
```

### Updating release to the latest version

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