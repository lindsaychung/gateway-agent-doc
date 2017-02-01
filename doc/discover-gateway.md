# Discover Gateway

Gateway-agent will be discoverable in the same LAN, when `advertiser` is enabled.
Your app needs to implement SSDP protocol to discover gateway-agent. Please refer the SSDP specification <https://tools.ietf.org/html/draft-cai-ssdp-v1-03>

## Enable Advertiser

To enable `advertiser`, you must add the following configuration in config.yml.
Please change `en0` depending on the real network environment.

```yaml
advertiser:
  interface: "en0"
```
gateway-agent will advertise SSDP service on `en0` network interface.

## How to Discover

1.  Multicast SSDP `M-SEARCH`

    Please configure the headers as following:

    * `MAN`: `"ssdp:discover"`
    * `ST` : `ssdp:all` or value configured in `service_type`.

2.  Handle response

    Value of `LOCATION` header of response is the base URL(IP address and port number)
    to access gateway-agent REST API.

    The response can be like this:

        HTTP/1.1 200 OK
        CACHE-CONTROL: max-age=1800
        LOCATION: http://192.168.1.100:4001/
        SERVER: darwin UPnP/1.1 gateway-agent
        ST: urn:kii:service:iot-gateway:1
        USN: uuid:ce6f2a98-6b57-46fd-851b-feb5fdf3fa8f::urn:kii:service:iot-gateway:1

## Configure Advertiser

You can configure other properties of `advertiser` in config.yml file than `interface`.  

### `interface` property

Configure a network interface name for SSDP advertising. If not configured, SSDP will be disabled.

### `service_type` property

The configured value will be as value of `ST` header in response.
It is optional. If not configured, `urn:kii:service:iot-gateway:1` will be used.
SSDP client can use this value to search gateway-agent only.

### `usn` property

The configured value will be as value of `USN` header in response.
It is optional. If not configured, it will be generated from `gateway.vendorThingID` and `service_type`, by the format: `uuid:{gateway.vendorThingID}::{service_type}`

### `max_age` property

The configured value will be used as value of `maxAge` in `CACHE-CONTROL` header in response.
It is optional. If not configured,`1800`(seconds) will be used.

### `alive_interval` property

Interval(seconds) to multicast SSDP alive message. It is optional. If not configured,
same value as `max_age` will be used.

## Advertiser Behaviors

If SSDP advertiser is enabled, gateway-agent will start to respond `M-SEARCH` request. The specified base URL to access REST APIs will be responded as value of `LOCATION` header.

A `ssdp:alive` message will be multicasted as soon as gateway-agent started. After that the SSDP alive message will be advertised based on the `alive_interval`.

When gateway-agent is stopped normally, it will multicast `ssdp:byebye` message.
