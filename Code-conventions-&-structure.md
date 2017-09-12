Following document describes Dashboard structure and code conventions. **It reflects only the structure as of version 1.6.x and may not reflect the structure of previous or future versions.**

## Structure

Dashboard is split into backend API and a frontend app. Backend API talks with Kubernetes API and contains most of the application logic, frontend is used to display all the data to the user in a friendly way.

### Backend

- Written in [Golang](https://golang.org/).
- Code and tests are stored in `src/app/backend` directory. Test file names start the same as sources, but they are with `_test.go`.
- Every API call hits `apihandler.go` which implements a series of handler functions to pass the results to resource-specific handlers.
- Backend currently doesn't implement a cache, so calls to the Dashboard API will always make fresh calls to the  Kubernetes API server.

### Frontend

- Code is stored in `src/app/frontend` directory.
- Tests are stored in `src/tests/frontend` directory. Directory structure should reflect `src/app/frontend` directory.
- The frontend makes calls to the api and renders received data. The frontend also transforms some data on the client and provides visualizations for the user. The frontend also makes calls to the api server to do things like exec into a container directly from the dashboard.

- The frontend also automatically generates localized translations. You can generate translations manually by running `gulp generate-xtbs` or `gulp serve:prod`. Take a look at the [internationalization guide](https://github.com/kubernetes/dashboard/wiki/Internationalization).

- The frontend uses [angular](https://angular.io/), a javascript model-view-controller framework along with [material design](https://material.angularjs.org/latest/getting-started) for cards, components, search bars, and all other visuals.

- [Material design guidelines](https://material.io/guidelines/)

- Production javascript is compiled via [google closure compiler](https://developers.google.com/closure/compiler/), so please make sure all functions and variables are properly type annotated.

### Tests
- The frontend tests can be found in the `src/test/` directory. That test directory is structured to mirror the `/src/app` directory.

- The backend tests can be found in the same directory as their source files in `src/app/backend`.

An overview of the features provided by the dashboard can be found [here](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard)

If you're looking for ideas on what to contribute, in addition to taking a look at issues with the `help-wanted` tag, you may also want to view the [Dashboard roadmap](https://github.com/kubernetes/dashboard/wiki/Roadmap)

If you have any further questions, feel free to ask in `#sig-ui` on the [kubernetes slack channel](https://kubernetes.slack.com).

## Conventions

### Backend

We are following conventions described in [Effective Go](https://golang.org/doc/effective_go.html) document.

Go to our [Go Report Card](https://goreportcard.com/report/github.com/kubernetes/dashboard) to check how well
we are doing.

### Frontend

We are following conventions described in [Angular 1 Style Guide](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md).

Check following subsections for more rules and tips.

#### Naming conventions

Here is a list of rules, that we follow:

- Self-explanatory names.
- Private method and variable names should end with a `_`.
 
Please notice, that this is not a list of all common programming rules. Use it as a list of tips designed for this project.

#### JavaScript Annotations

We are using [Closure Compiler](https://developers.google.com/closure/compiler/) and therefore we need to match few requirements. One of them is proper usage of annotations which is described [here](https://github.com/google/closure-compiler/wiki/Annotating-JavaScript-for-the-Closure-Compiler).

#### Tooltips

In order to keep all tooltips consistent across whole application, we have decided to use 500 ms delay and auto-hide option. It allows us to avoid flickering when moving mouse over the pages and to hide tooltips after mouse is elsewhere but focus is still on the element with tooltip.

Sample code:

``` html
<md-tooltip md-delay="500" md-autohide>
   ...
</md-tooltip>
```