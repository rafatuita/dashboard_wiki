## Go dependencies

We are using [`dep`](https://github.com/golang/dep) as our Go dependency management tool. It is the official prototype, that most probably will become the official tool in the near future.

In order to speed up the whole development process, we have decided to keep dependency files inside our repository. `.gitignore` file helps us to ignore redundant files like tests etc.

### Updating dependencies

1. Have the Dashboard checked out in `${GOPATH}/src/github.com/kubernetes/dashboard`.
2. Have [`dep`](https://github.com/golang/dep) installed.
3. Create new branch.
4. Update dependency versions in `Gopkg.toml`.
5. Run `dep ensure` to update dependencies. Add `-update` parameter to update dependencies within their current version range.
6. Run `dep prune` to keep only used files.
7. Build and run the unit tests. If they are broken most likely some of our vendor packages have
changed API and it needs to be fixed.
8. Commit the changes in `vendor` directory and in our sources separately for easier review. It is important to not remove `.gitignore` file at this point.
9. Send a pull request.

## JavaScript dependencies

We are using [npm](https://www.npmjs.com/) as a JavaScript package manager.

TBD Greenkeeper

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
