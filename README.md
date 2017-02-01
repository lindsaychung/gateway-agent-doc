# Kii Gateway Agent

## Download
Download [prebuilt gateway agent](https://bintray.com/kii/kii-gateway-agent/gateway-agent#files)

Extract archive.
```
tar zxf artifacts-1.5.1.tar.gz
```

It contains
- config.yml
- binaries built for linux.
- binaries built for windows.

## How to run

### Prepare config file

Gateway agent will load `config.yml` located in the same directory.
You may need to modify it.

**Sections**:

- **master_app**: Contains the information of gateway app, created in Kii Cloud Platform.

- **gateway**: Contains device informations of gateway hardware. These informations are necessary when onboarding it to Kii Cloud.

- **admin**: Contains the necessary information to authorize mobile app to access REST API of gateway agent locally. When mobile app request token from gateway agent, `username` and `password` in request body must same the info configured in this section.

- **local_server**: Contains the necessary information when running local server of REST API locally.

- **mqtt**: Contains the necessary information when starts local MQTT broker for converter and creates MQTT client for receiving commands of end-node belongs to this gateway from Kii Cloud.

- **store**: Contains the informations of storage of gateway agent.

### Run
After you've finished config.yml modification,
Place gwagent-{arch} and config.yml in the same directory of your running environment.
Then execute the gwagent-{arch}

Linux example:
```shell
./gwagent-{arch}
```

This starts one HTTP server with port 4001.  Port 4001 is used for local
server, which provides REST API for applications.

## Run in Restore mode

Linux example:
```shell
./gwagent-{arch} -restore
```

## Files

* config.yml - configuration file. when absent, default values are used.
* props.yml  - generated configuration at first time.
* gwagent.db - database file.

When you want to reset the gateway-agent,
Remove props.yml and gwagent.db file before starting gateway.
(Equivalent to factory reset.)

## Tutrial

Please refer to [Getting Started](./doc/getting-started.mkd)
