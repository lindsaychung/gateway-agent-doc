# MQTTS (TLS) Support

This document describes MQTT broker over TLS (MQTTS) which supported by
gateway-agent.


## Getting Started

Add configurations of cert and key files to your config.yml:

```yaml
mqtt:
  local_x509_cert: "doc/tls/server.crt"
  local_x509_key:  "doc/tls/server.key"
```

These files are self signed certificate for develop and test.
YOU MUST REPLACE THOSE WHEN RELEASE PRODUCTS!


## Connect to MQTTS

To connect MQTTS which installed with self signed certificate, don't forget to
allow insecure host.  For example of golang:

```go
import "crypto/tls"
import "github.com/koron/go-mqtt/client"

c, err := client.Connect(client.Param{
	Options: &client.Options{
		TLSConfig: &tls.Config{
			// Allow insecure host for develop and test.
			InsecureSkipVerify: true,
		},
	},
})
```

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

*   When enable MQTTS broker, normal unsecure MQTT broker is disabled.
