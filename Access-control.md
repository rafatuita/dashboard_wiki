Once Dashboard is installed and accessible we can focus on configuring access control to the cluster resources for users. As of release 1.7 Dashboard no longer has full admin privileges granted by default. All the privileges are revoked and only minimal privileges granted, that are required to make Dashboard work.

In case Dashboard is accessible only by trusted set of people, all with full admin privileges you may want to grant it full [admin privileges](#admin-privileges). Note that other applications should not access Dashboard directly as it may cause privilege escalation. Make sure that in-cluster traffic is restricted to namespaces or just revoke access to Dashboard for other applications inside the cluster.

## Introduction

Kubernetes supports few ways of authenticating and authorizing users. You can read about them [here](https://kubernetes.io/docs/admin/authentication) and [here](https://kubernetes.io/docs/admin/authorization). Authorization is handled by Kubernetes API server. Dashboard only acts as a proxy and passes all auth information to it. In case of forbidden access corresponding warnings will be displayed in Dashboard.

## Authentication

As of release 1.7 Dashboard supports user authentication based on:
- [`Authorization: Bearer <token>`](#authorization-header) header passed in every request to Dashboard. Supported from release 1.6.
- [Bearer Token](#bearer-token) that can be used on Dashboard [login view](#login-view).
- [Username/password](#basic) that can be used on Dashboard [login view](#login-view).
- [Kubeconfig](#kubeconfig) file that can be used on Dashboard [login view](#login-view). Note that kubeconfig file has to be configured to use either Bearer Token or username/password. External identity providers or certificate-based authentication are not supported at this time.

### Login view
Login view has been introduced in release 1.7. In order to make it appear in Dashboard you need to enable and access Dashboard over HTTPS. To do so you need to pass `--tls-cert-file` and `--tls-cert-key` flags to Dashboard. HTTPS endpoint will be exposed on port `8443` in Dashboard container. You can change it by providing `--port` flag.

Using `Skip` option will make Dashboard use privileges of Service Account used by Dashboard.

![zrzut ekranu z 2017-08-31 13-23-42](https://user-images.githubusercontent.com/2285385/29920823-a5c7f8d2-8e4f-11e7-9127-e858909688f6.png)

### Authorization header

Using authorization header is the only way to make Dashboard act as an user, when accessing it over HTTP. Note that there are some risks since plain HTTP traffic is vulnerable to [MITM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) attacks.

To make Dashboard use authorization header you simply need to pass `Authorization: Bearer <token>` in every request to Dashboard. This can be achieved i.e. by configuring reverse proxy in front of Dashboard. Proxy will be responsible for authentication with identity provider and will pass generated token in request header to Dashboard. Note that Kubernetes API server needs to be configured properly to accept these tokens.

To quickly test it you could use some browser plugin similar to [Requestly](https://chrome.google.com/webstore/detail/requestly-redirect-url-mo/mdnleldcmiljblolnjhpnblkcekpdkpa) that allows to manually modify request headers.

### Bearer Token

### Basic

### Kubeconfig

## Admin privileges