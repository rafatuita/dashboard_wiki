Dashboard container accepts multiple arguments that can be used to customize it a bit. In example we are using `--tls-key-file` and `--tls-cert-file` flags in our recommended setup [YAML files](https://github.com/kubernetes/dashboard/blob/master/src/deploy/recommended/kubernetes-dashboard.yaml#L114) to pass certificates to Dashboard.

## Arguments

| Argument name              | Default value | Description                                                                                                                                                                                                                                         |
|----------------------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| insecure-port              | 9090          | The port to listen to for incoming HTTP requests.                                                                                                                                                                                                    |
| port                       | 8443          | The secure port to listen to for incoming HTTPS requests.                                                                                                                                                                                           |
| insecure-bind-address      | 127.0.0.1     | The IP address on which to serve the --port (set to 0.0.0.0 for all interfaces).                                                                                                                                                                    |
| bind-address               | 0.0.0.0       | The IP address on which to serve the --secure-port (set to 0.0.0.0 for all interfaces).                                                                                                                                                             |
| tls-cert-file              | -             | File containing the default x509 Certificate for HTTPS.                                                                                                                                                                                             |
| tls-key-file               | -             | File containing the default x509 private key matching --tls-cert-file.                                                                                                                                                                              |
| apiserver-host             | -             | The address of the Kubernetes Apiserver to connect to in the format of protocol://address:port, e.g., http://localhost:8080. If not specified, the assumption is that the binary runs inside a Kubernetes cluster and local discovery is attempted. |
| heapster-host              | -             | The address of the Heapster to connect to in the format of protocol://address:port, e.g., http://localhost:8082. If not specified, the assumption is that the binary runs inside a Kubernetes cluster and service proxy will be used.     |
| kubeconfig                 | -             | Path to kubeconfig file with authorization and master location information.                                                                                                                                                                         |
| token-ttl                  | 15 minutes    | Expiration time (in seconds) of JWE tokens generated by dashboard. Default: 15 min. 0 - never expires.                                                                                                                                               |
| authentication-mode        | token         | Enables authentication options that will be reflected on login screen. Supported values: token, basic. Default: token. Note that basic option should only be used if apiserver has '--authorization-mode=ABAC' and '--basic-auth-file' flags set.   |
| metric-client-check-period | 30 seconds    | Time in seconds that defines how often configured metric client health check should be run. Default: 30 seconds.                                                                                                                                    |