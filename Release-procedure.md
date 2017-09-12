So you want to release a new version of Kubernetes Dashboard? Great, you just need to follow
the steps below.

## Release procedure

1. Test everything twice on Docker image and `gulp serve:prod`.
2. Send a pull request that increases version numbers in all files. Follow versioning guidelines. Files to keep in sync are listed below:
   - `bower.json`
   - `package.json`
   - `build/conf.js`
   - YAML files from `src/deploy`
3. Get the pull request reviewed and merged.
4. Create a git [release tag](https://github.com/kubernetes/dashboard/releases/) for the merged pull request. Release description should include a changelog.
5. Build and push production images to container registry. Use `gulp push-to-gcr:release`.
6. Update addons on the Kubernetes core repository. Dashboard addon directory is [here](https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/dashboard). If the update is minor, all that needs to be done is to change image version number in the main controller config file (`dashboard-controller.yaml`), and other configs, as described in the header of the config. If the release is major, this needs coordination with Kubernetes core team and possibly alignment with the schedule of the core.
7. Also, update addon config in the [minikube](https://github.com/kubernetes/minikube/tree/master/deploy/addons) repo.

## Versioning Guidelines

Kubernetes Dashboard versioning follows [semver](http://semver.org/) in spirit. This means
that is uses `vMAJOR.MINOR.PATCH` version numbers, but uses UX and consumer-centric approach for
incrementing version numbers.

1. Increment MAJOR when there are breaking changes that affect user's workflows or the UX gets
   major redesign.
1. Increment MINOR when new functionality is added or there are minor UX changes.
1. Increment PATCH in case of bug fixes and minor new features.

Versions `0.X.Y` are reserved for initial development and may not strictly follow the guidelines.