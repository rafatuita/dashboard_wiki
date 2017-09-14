In case you did not find any answer here and in [closed issues](https://github.com/kubernetes/dashboard/issues?q=is%3Aissue+is%3Aclosed), [create new issue](https://github.com/kubernetes/dashboard/issues/new).

###  During development I receive a lot of strange errors in the browser's console. What may be wrong?

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

