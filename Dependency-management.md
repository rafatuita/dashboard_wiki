## Go dependencies

We are using [`dep`](https://github.com/golang/dep) as our Go dependency management tool. It is the official prototype, that most probably will become the official tool in the near future.

In order to speed up the whole development process, we have decided to keep dependency files inside our repository. `.gitignore` file helps us to ignore redundant files like tests etc.

### Updating dependencies

Go dependency versions are stored in `Gopkg.toml` file. Its [format](https://github.com/golang/dep/blob/master/docs/Gopkg.toml.md) should be similar to one presented below:

```toml
[[constraint]]
  name = "k8s.io/client-go"
  version = "v5.0.0"

[[override]]
  name = "github.com/emicklei/go-restful"
  version = "1.0.0"
```

To update dependency version edit `Gopkg.toml` file and then run `dep ensure` to download dependencies. Adding `-update` parameter will update dependencies within their current version range too. In the end `dep prune` should be run to remove not used dependencies.

**NOTES:** 

- In order to work with Go dependencies repository should be checked out in `${GOPATH}/src/github.com/kubernetes/dashboard` directory.
- Commit the changes in `vendor` directory and in our sources separately for easier review. It is important to not remove `.gitignore` in any of commits.

## JavaScript dependencies

We are using [`npm`](https://www.npmjs.com/) as a JavaScript package manager.

In order to keep our dependencies up-to-date we have introduced [Greenkeeper](https://greenkeeper.io/) bot to the project. It scans for new dependency versions and automatically creates pull requests for them. Greenkeeper's pull request are marked with `greenkeeper` label.

JavaScript dependencies are kept outside repository thanks to `.gitignore` file. In order to start developing it is needed to run `npm i` after checking out the repository.

### Checking for updates

Run `gulp check-npm-deps` to check for updates. The output should be similar to one presented below:

```
$ gulp check-npm-deps
[10:51:05] Requiring external module babel-register
[10:51:07] Using gulpfile ~/go/src/github.com/kubernetes/dashboard/gulpfile.babel.js
[10:51:07] Starting 'check-npm-deps'...
[10:51:12] Dependencies needed to update:
@uirouter/core: ~5.0.11
@uirouter/angularjs: ~1.0.10
angular-clipboard: ~1.6.2
angular-material: ~1.1.5
d3: ~4.11.0
hterm: ~2.0.2
babel-preset-env: ~1.6.1
browserify-istanbul: ~3.0.1
eslint-plugin-angular: ~3.1.1
gulp-sass-lint: ~1.3.4
gulp-useref: ~3.1.3
html-minifier: ~3.5.6
karma-sauce-launcher: ~1.2.0
q: ~1.5.1

Run: 'gulp update-npm-deps', then 'npm install' to update dependencies.

[10:51:12] Finished 'check-npm-dependencies' after 4.7 s
```

### Updating dependencies

TBD
