# HTTPS Support

This document describes HTTPS support on local server.

## Getting Started

Add configurations of cert and key files to your config.yml:

```yaml
loca_server:
  cert: "doc/https/server.crt"
  key:  "doc/https/server.key"
```

These files are self signed certificate for develop and test.
YOU MUST REPLACE THOSE WHEN RELEASE PRODUCTS!

## Connect to Local Server

To connect to local server which have self signed certificate by using curl,
try `-k` option.

    $ curl -k http://127.0.0.1:4001/gateway-info

DON'T USE INSECURE SETTINGS FOR REAL PRODUCTS!

## TLS self signed certificate

This directory includes TLS certificate related files for develop and test.

How to generate those are:

    openssl genrsa -out server.key 2048
    openssl req -new -key server.key -out server.csr
    openssl x509 -days 9999 -req -signkey server.key -in server.csr -extfile server.cnf -out server.crt

Current subject is:

    /C=JP/ST=Tokyo/L=Minato-ku/O=Kii Corporation/OU=Development/CN=gateway-agent

## Remarks

*   When enable HTTPS local server, unsecure HTTP server will be disabled.
