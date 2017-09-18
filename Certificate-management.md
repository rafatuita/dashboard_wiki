This document describes shortly how to get certificates, that can be used to enable HTTPS in Dashboard.

## Public trusted Certificate Authority

There are many public and free certificate providers to choose from. One of the best trusted certificate providers is [Let's encrypt](https://letsencrypt.org/). Everything you need to know about how to generate certificates signed by their trusted CA can be found [here](https://letsencrypt.org/getting-started/).

## Self-signed certificate

In case you want to generate certificates on your own you need library like [OpenSSL](https://www.openssl.org/) that will help you do that.

### Generate private key and certificate signing request

A private key and certificate signing request are required to create an SSL certificate. These can be generated with a few simple commands.
When the openssl req command asks for a “challenge password”, just press return, leaving the password empty. This password is used by Certificate Authorities to authenticate the certificate owner when they want to revoke their certificate. Since this is a self-signed certificate, there’s no way to revoke it via CRL (Certificate Revocation List).

```bash
$ openssl genrsa -des3 -passout pass:x -out dashboard.pass.key 2048
...
$ openssl rsa -passin pass:x -in dashboard.pass.key -out dashboard.key
# Writing RSA key
$ rm dashboard.pass.key
$ openssl req -new -key dashboard.key -out dashboard.csr
...
Country Name (2 letter code) [AU]: US
...
A challenge password []:
...
```

### Generate SSL certificate

The self-signed SSL certificate is generated from the `dashboard.key` private key and `dashboard.csr` files.

```bash
$ openssl x509 -req -sha256 -days 365 -in dashboard.csr -signkey dashboard.key -out dashboard.crt
```

The `dashboard.crt` file is your certificate suitable for use with Dashboard along with the `dashboard.key` private key.