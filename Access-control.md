Once Dashboard is installed and accessible we can focus on configuring access control to the cluster resources for users. As of release 1.7 Dashboard no longer has full admin privileges granted by default. All the privileges are revoked and only [minimal privileges granted](#default-dashboard-privileges), that are required to make Dashboard work.

**IMPORTANT** 

Below note is only directed to people using Dashboard 1.7 and above.

In case Dashboard is accessible only by trusted set of people, all with full admin privileges you may want to grant it full [admin privileges](#admin-privileges). Note that other applications should not access Dashboard directly as it may cause privileges escalation. Make sure that in-cluster traffic is restricted to namespaces or just revoke access to Dashboard for other applications inside the cluster.

**IMPORTANT**

## Introduction

Kubernetes supports few ways of authenticating and authorizing users. You can read about them [here](https://kubernetes.io/docs/admin/authentication) and [here](https://kubernetes.io/docs/admin/authorization). Authorization is handled by Kubernetes API server. Dashboard only acts as a proxy and passes all auth information to it. In case of forbidden access corresponding warnings will be displayed in Dashboard.

## Default Dashboard privileges

- `create` and `watch` permission for secrets in `kube-system` namespace required to create and watch for changes of `kubernetes-dashboard-key-holder` secret.
- `get`, `update` and `delete` permissions for secret named `kubernetes-dashboard-key-holder` in `kube-system` namespace.
- `proxy` permission to `heapster` service in `kube-system` namespace required to allow getting metrics from heapster.

## Authentication

As of release 1.7 Dashboard supports user authentication based on:
- [`Authorization: Bearer <token>`](#authorization-header) header passed in every request to Dashboard. Supported from release 1.6. Has the highest priority. If present, login view will not be shown.
- [Bearer Token](#bearer-token) that can be used on Dashboard [login view](#login-view).
- [Username/password](#basic) that can be used on Dashboard [login view](#login-view).
- [Kubeconfig](#kubeconfig) file that can be used on Dashboard [login view](#login-view).

### Login view
Login view has been introduced in release 1.7. In order to make it appear in Dashboard you need to enable and access Dashboard over HTTPS. To do so you need to pass `--tls-cert-file` and `--tls-cert-key` flags to Dashboard. HTTPS endpoint will be exposed on port `8443` of Dashboard container. You can change it by providing `--port` flag.

Using `Skip` option will make Dashboard use privileges of Service Account used by Dashboard.

![zrzut ekranu z 2017-09-14 09-17-02](https://user-images.githubusercontent.com/2285385/30416718-8ee657d8-992d-11e7-84c8-9ba5f4c78bb2.png)

### Authorization header

Using authorization header is the only way to make Dashboard act as an user, when accessing it over HTTP. Note that there are some risks since plain HTTP traffic is vulnerable to [MITM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) attacks.

To make Dashboard use authorization header you simply need to pass `Authorization: Bearer <token>` in every request to Dashboard. This can be achieved i.e. by configuring reverse proxy in front of Dashboard. Proxy will be responsible for authentication with identity provider and will pass generated token in request header to Dashboard. Note that Kubernetes API server needs to be configured properly to accept these tokens.

To quickly test it check out [Requestly](https://chrome.google.com/webstore/detail/requestly-redirect-url-mo/mdnleldcmiljblolnjhpnblkcekpdkpa) browser plugin that allows to manually modify request headers.

**IMPORTANT**: Authorization header will not work if Dashboard is accessed through API server proxy. Both `kubectl proxy` and `API Server` way of accessing Dashboard described in [Accessing Dashboard](https://github.com/kubernetes/dashboard/wiki/Accessing-dashboard) guide will not work. It is due to the fact that once request reaches API server all additional headers are dropped. You can track proposal that was supposed to make it work [here](https://github.com/kubernetes/kubernetes/pull/29714).

### Bearer Token

Once you are in possession of valid Bearer Token (accepted by Kubernetes API server), it can be used to log in to Dashboard. In example every Service Account has Secret with valid token that can be used to log in.

![zrzut ekranu z 2017-09-13 11-29-36](https://user-images.githubusercontent.com/2285385/30370159-09af99aa-9877-11e7-8cb6-28fb9af88c83.png)

### Basic

Basic authentication is disabled by default. The reason is that Kubernetes API server needs to be configured with authorization mode ABAC and `--basic-auth-file` flag provided. Without that API server automatically falls back to [anonymous user](https://kubernetes.io/docs/admin/authentication/#anonymous-requests) and there is no way to check if provided credentials are valid.

In order to enable basic auth in Dashboard `--authentication-mode=basic` flag has to be provided. By default it is set to `--authentication-mode=token`.

### Kubeconfig

This method of logging in is provided for convenience. Only authentication options specified by `--authentication-mode` flag are supported in kubeconfig file. In case it is configured to use any other way, error will be shown in Dashboard. External identity providers or certificate-based authentication are not supported at this time.

![zrzut ekranu z 2017-08-31 13-28-38](https://user-images.githubusercontent.com/2285385/29920994-5214087e-8e50-11e7-8ab9-c75755b62a47.png)

## Admin privileges

TODO