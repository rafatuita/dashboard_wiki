Following document describes Dashboard code structure and conventions. **It reflects only the structure as of version 1.6.x and may not reflect the structure of previous or future versions.**

## Structure

Dashboard is split into backend API and a frontend app. Backend API talks with Kubernetes API and contains most of the application logic, frontend is used to display all the data to the user in a friendly way.

### Backend

- Written in [Golang](https://golang.org/).
- Code and tests are stored in `src/app/backend` directory. Test file names start the same as sources, but they are with `_test.go`.
- Every API call hits `apihandler.go` which implements a series of handler functions to pass the results to resource-specific handlers.
- Backend currently doesn't implement a cache, so calls to the Dashboard API will always make fresh calls to the  Kubernetes API server.

### Frontend

- Written in JavaScript.
- Using [AngularJS](https://github.com/angular/angular.js), a model-view-controller along with [Material Design](https://material.angularjs.org/latest/getting-started) for cards, components, search bars, and all other visuals.
- Production JavaScript is compiled via [Google Closure Compiler](https://developers.google.com/closure/compiler/), so please make sure all functions and variables are properly type annotated. You should follow [this](https://github.com/google/closure-compiler/wiki/Annotating-JavaScript-for-the-Closure-Compiler) guide.
- Code is stored in `src/app/frontend` directory.
- Tests are stored in `src/tests/frontend` directory. Directory structure should reflect `src/app/frontend` directory.
- Frontend makes calls to the API and renders received data. It also transforms some data on the client and provides visualizations for the user. The frontend also makes calls to the API server to do things like exec into a container directly from the dashboard.

An overview of the features provided by the dashboard can be found [here](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard)

If you're looking for ideas on what to contribute, in addition to taking a look at issues with the `help-wanted` tag, you may also want to view the [Dashboard roadmap](https://github.com/kubernetes/dashboard/wiki/Roadmap)

If you have any further questions, feel free to ask in `#sig-ui` on the [kubernetes slack channel](https://kubernetes.slack.com).

## Conventions

### General

- Use self-explanatory names in the code.

### Backend

We are following conventions described in [Effective Go](https://golang.org/doc/effective_go.html) document.

Go to our [Go Report Card](https://goreportcard.com/report/github.com/kubernetes/dashboard) to check how well
we are doing.

### Frontend

We are following conventions described in [Angular 1 Style Guide](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md) and [Material Design Guidelines](https://material.io/guidelines/).

Additionally, check list of rules and tips, that we are using:

- Private method and variable names should end with a `_`.
- In order to keep all tooltips consistent across whole application, we have decided to use 500 ms delay and auto-hide option. It allows us to avoid flickering when moving mouse over the pages and to hide tooltips after mouse is elsewhere but focus is still on the element with tooltip.