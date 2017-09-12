## Dashboard integrations

Currently we are supporting only [Heapster](https://github.com/kubernetes/heapster) integration, however we are working on introducing integration framework to Dashboard. It will allow to support and integrate more metric providers as well as additional applications such as [Weave Scope](https://github.com/weaveworks/scope) or [Grafana](https://github.com/grafana/grafana). 

### Metric integrations

Metric integrations allow Dashboard to show cpu and memory usage graphs, and sparklines of resources running inside the cluster.

#### Heapster

For the metrics and graphs to be available you need to have [Heapster](https://github.com/kubernetes/heapster/) running on your cluster. We require heapster to be deployed in `kube-system` namespace together with service named `heapster`. In case heapster is not accessible from inside the cluster you can provide heapster url as a flag to Dashboard container `--heapster-host=<heapster_url>`.

**NOTE:** Currently `--heapster-host` flag does not support HTTPS connection. Only HTTP urls should be used.