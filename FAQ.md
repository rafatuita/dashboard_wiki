### During development I receive a lot of strange errors in the browser's console. What is wrong?

You probably need to update your npm dependencies. To do it run following commands from Dashboard's root directory:

```
rm -rf node_modules
rm -rf bower_components
npm i
```

### Why my `Go is not in the path`?

Running into an error like that probably means, that you need to rerun `export PATH=$PATH:/usr/local/go/bin`.




If you have an error like "linux mounts: Path /var/lib/kubelet is mounted on / but it is not a shared mount." you should try `sudo mount --bind /var/lib/kubelet /var/lib/kubelet` followed by `sudo mount --make-shared /var/lib/kubelet`. ([source](https://github.com/kubernetes/kubernetes/issues/4869#issuecomment-193640483))


