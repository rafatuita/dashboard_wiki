This document describes how to setup your development environment.

## Preparation

Make sure the following software is installed and added to the `$PATH` variable:

* Docker 1.10+ ([installation manual](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/))
* Golang 1.10.3+ ([installation manual](https://golang.org/dl/))
* Node.js 9+ and npm 6+ ([installation with nvm](https://github.com/creationix/nvm#usage))
* Gulp.js 4+ ([installation manual](https://github.com/gulpjs/gulp/blob/master/docs/getting-started/1-quick-start.md))

Clone the repository into `$GOPATH/src/github.com/kubernetes/dashboard` and install the dependencies:

```shell
$ npm ci
```

If you are running commands with root privileges set `--unsafe-perm` flag:

 ```shell
 npm ci --unsafe-perm
 ```

## Running the cluster

To make Dashboard work you need to have cluster running. If you would like to use local cluster we recommend [kubeadm](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/), [minikube](https://kubernetes.io/docs/getting-started-guides/minikube/) or [kubeadm-dind-cluster](https://github.com/Mirantis/kubeadm-dind-cluster). The most convenient way is to make it work is to create a proxy. Run the following command:

```shell
$ kubectl proxy
```

kubectl will handle authentication with Kubernetes and create an API proxy with the address
`localhost:8080`. Therefore, no changes in the configuration are required.

Another way to connect to real cluster while developing dashboard is to override default values used
by our build pipeline. In order to do that we have introduced two environment variables
`KUBE_DASHBOARD_APISERVER_HOST` and `KUBE_DASHBOARD_KUBECONFIG` that will be used over default ones when
defined. Before running our npm tasks just do:

```shell
$ export KUBE_DASHBOARD_APISERVER_HOST="http://<APISERVER_IP>:<APISERVER_PORT>"
# or
$ export KUBE_DASHBOARD_KUBECONFIG="<KUBECONFIG_FILE_PATH>"
```

**NOTE: Environment variable `KUBE_DASHBOARD_KUBECONFIG` has higher priority than `KUBE_DASHBOARD_APISERVER_HOST`.**

## Serving Dashboard for Development

Quick updated version:

```shell
npm start
```

Open a browser and access the UI under `localhost:8080`.

In the background, `npm start` makes a [concurrently](https://github.com/kimmobrunfeldt/concurrently#readme) call to start the `golang` backend server and the `angular` development server.

Once the `angular` server starts, it takes some time to pre-compile all assets before serving them. By default, the angular development server watches for file changes and will update accordingly.

## Building Dashboard for production

The Dashboard project can be built for production by using the following task:

```shell
$ npm run build
```

The code is compiled, compressed and debug support removed. The dashboard binary can be found
in the `dist` folder.

To build and immediately serve Dashboard from the `dist` folder, use the following task:

```shell
$ npm run start:prod
```

Open a browser and access the UI under `localhost:9090.` The following processes should
be running (respective ports are given in parentheses):


Dashboard backend (9090)  ---> Kubernetes API server (8080)



In order to package everything into a ready-to-run Docker image, use the following task:

```shell
$ npm run docker:build:head
```

You might notice that the Docker image is very small and requires only a few MB. Only
Dashboard assets are added to a scratch image. This is possible, because the `dashboard`
binary has no external dependencies. Awesome!

## Run the tests

Unit tests should be executed after every source code change. The following task makes this
a breeze by automatically executing the unit tests after every save action.

```shell
$ npm run test
```

The full test suite includes static code analysis, unit tests and integration tests.
It can be executed with:

```shell
$ npm run check
```

You can also run individual tests on their own (such as the backend or frontend tests) by doing the following:

```shell
$ npm run test:frontend
$ npm run test:backend
```

## Committing changes to your fork

Before committing any changes, please run `npm run check`.
This will keep you from accidentally committing non tested and unformatted code.

Since the hooks for commit has been set with `husky` into
`<dashboard_home>/.git/hooks/pre-commit` already if you installed dashboard
according to above, so it will keep your code as formatted.

Then you can commit your changes and push them to your fork:

```shell
$ git commit
$ git push -f origin my-feature
```

## Building dashboard inside a container

To run dashboard using docker at ease, 

1. Copy kubeconfig from your cluster, and confirm the URL for API server in it, and modify it if necessary.
2. Set filepath for kubeconfig into `K8S_DASHBOARD_KUBECONFIG` environment variable.
3. Change directory into your dashboard source directory.
4. Run `aio/develop/run-npm-on-container.sh`.

The container will build and run dashboard as default. Also, you can run npm commands described in `package.json`. e.g. To run code check:
`aio/develop/run-npm-on-container.sh run check`



