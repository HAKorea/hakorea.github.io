---
title: HTTP
description: Offers a web framework to serve files.
logo: http.png
ha_category:
  - Other
  - Binary Sensor
  - Sensor
ha_release: pre 0.7
ha_iot_class: Local Push
ha_quality_scale: internal
ha_codeowners:
  - '@home-assistant/core'
---

The `http` integration serves all files and data required for the Home Assistant frontend. You only need to add this to your configuration file if you want to change any of the default settings.

There is currently support for the following device types within Home Assistant:

- [Binary Sensor](#binary-sensor)
- [Sensor](#sensor)

<div class='note'>

Don't use option `server_host` on a Hass.io installation!

</div>

```yaml
# Example configuration.yaml entry
http:
```

{% configuration %}
server_host:
  description: "Only listen to incoming requests on specific IP/host. By default it will accept all IPv4 connections. Use `server_host: ::0` if you want to listen to (and only) IPv6."
  required: false
  type: string
  default: 0.0.0.0
server_port:
  description: Let you set a port to use.
  required: false
  type: integer
  default: 8123
base_url:
  description: "The URL that Home Assistant is available on the internet. For example: `https://hass-example.duckdns.org:8123`. The iOS app finds local installations, if you have an outside URL use this so that you can auto-fill when discovered in the app. Note that this setting may only contain a protocol, hostname and port; using a path is *not* currently supported."
  required: false
  type: string
  default: Your local IP address
ssl_certificate:
  description: Path to your TLS/SSL certificate to serve Home Assistant over a secure connection.
  required: false
  type: string
ssl_peer_certificate:
  description: Path to the client/peer TLS/SSL certificate to accept secure connections from.
  required: false
  type: string
ssl_key:
  description: Path to your TLS/SSL key to serve Home Assistant over a secure connection.
  required: false
  type: string
cors_allowed_origins:
  description: "A list of origin domain names to allow [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) requests from. Enabling this will set the `Access-Control-Allow-Origin` header to the Origin header if it is found in the list, and the `Access-Control-Allow-Headers` header to `Origin, Accept, X-Requested-With, Content-type, Authorization`. You must provide the exact Origin, i.e. `https://www.home-assistant.io` will allow requests from `https://www.home-assistant.io` but __not__ `http://www.home-assistant.io`."
  required: false
  type: [string, list]
use_x_forwarded_for:
  description: "Enable parsing of the `X-Forwarded-For` header, passing on the client's correct IP address in proxied setups. You **must** also whitelist trusted proxies using the `trusted_proxies` setting for this to work. Non-whitelisted requests with this header will be considered IP spoofing attacks, and the header will, therefore, be ignored."
  required: false
  type: boolean
  default: false
trusted_proxies:
  description: "List of trusted proxies, consisting of IP addresses or networks, that are allowed to set the `X-Forwarded-For` header.  This is required when using `use_x_forwarded_for` because all requests to Home Assistant, regardless of source, will arrive from the reverse proxy IP address. Therefore in a reverse proxy scenario, this option should be set with extreme care."
  required: false
  type: [string, list]
trusted_networks:
  description: "**Deprecated since 0.89 release. Configuration moved to [Trusted Networks auth provider](/docs/authentication/providers/#trusted-networks).** List of trusted networks, consisting of IP addresses or networks, that are allowed to bypass password protection when accessing Home Assistant.  If using a reverse proxy with the `use_x_forwarded_for` and `trusted_proxies` options enabled, requests proxied to Home Assistant with a trusted `X-Forwarded-For` header will appear to come from the IP given in that header instead of the proxy IP."
  required: false
  type: [string, list]
ip_ban_enabled:
  description: Flag indicating whether additional IP filtering is enabled.
  required: false
  type: boolean
  default: false
login_attempts_threshold:
  description: "Number of failed login attempt from single IP after which it will be automatically banned if `ip_ban_enabled` is `true`. When set to -1 no new automatic bans will be added."
  required: false
  type: integer
  default: -1
ssl_profile:
  description: The [Mozilla SSL profile](https://wiki.mozilla.org/Security/Server_Side_TLS) to use. Only lower if you are experiencing integrations causing SSL handshake errors.
  required: false
  type: string
  default: modern
{% endconfiguration %}

<div class='note'>

Configuring trusted_networks via the `http` integration will be deprecated and moved to `auth_providers` instead. For instructions, see <a href="/docs/authentication/providers/#trusted-networks">trusted networks</a>. In Home Assistant 0.89.0 and 0.89.1, you need place the trusted network under both `http` and `auth_providers` if you still want to use trusted networks features. You can remove it from `http` section starting from 0.89.2.

</div>

The sample below shows a configuration entry with possible values:

```yaml
# Example configuration.yaml entry
http:
  server_port: 12345
  ssl_certificate: /etc/letsencrypt/live/hass.example.com/fullchain.pem
  ssl_key: /etc/letsencrypt/live/hass.example.com/privkey.pem
  cors_allowed_origins:
    - https://google.com
    - https://www.home-assistant.io
  use_x_forwarded_for: true
  trusted_proxies:
    - 10.0.0.200
  ip_ban_enabled: true
  login_attempts_threshold: 5
```

The [Set up encryption using Let's Encrypt](/blog/2015/12/13/setup-encryption-using-lets-encrypt/) blog post gives you details about the encryption of your traffic using free certificates from [Let's Encrypt](https://letsencrypt.org/).

Or use a self signed certificate following the instructions here [Self-signed certificate for SSL/TLS](/docs/ecosystem/certificates/tls_self_signed_certificate/).

## APIs

On top of the `http` integration is a [REST API](/developers/rest_api/), [Python API](/developers/python_api/) and [WebSocket API](/developers/websocket_api/) available. There is also support for [Server-sent events](/developers/server_sent_events/).

The `http` platforms are not real platforms within the meaning of the terminology used around Home Assistant. Home Assistant's [REST API](/developers/rest_api/) sends and receives messages over HTTP.

## HTTP sensors

To use those kind of [sensors](#sensor) or [binary sensors](#binary-sensor) in your installation no configuration in Home Assistant is needed. All configuration is done on the devices themselves. This means that you must be able to edit the target URL or endpoint and the payload. The entity will be created after the first message has arrived.

Create a [Long-Lived Access Tokens](https://developers.home-assistant.io/docs/en/auth_api.html#long-lived-access-token) in the Home Assistant UI at the bottom of your profile if you want to use HTTP sensors.

All [requests](/developers/rest_api/#post-apistatesltentity_id) need to be sent to the endpoint of the device and must be **POST**.

## IP filtering and banning

If you want to apply additional IP filtering, and automatically ban brute force attempts, set `ip_ban_enabled` to `true` and the maximum number of attempts. After the first ban, an `ip_bans.yaml` file will be created in the root configuration folder. It will have the banned IP address and time in UTC when it was added:

```yaml
127.0.0.1:
  banned_at: '2016-11-16T19:20:03'
```

After a ban is added a Persistent Notification is populated to the Home Assistant frontend.

<div class='note warning'>

Please note, that sources from `trusted_networks` won't be banned automatically.

</div>

## Hosting files

If you want to use Home Assistant to host or serve static files then create a directory called `www` under the configuration path (`/config` on Hass.io, `.homeassistant` elsewhere). The static files in `www/` can be accessed by the following URL `http://your.domain:8123/local/`, for example `audio.mp3` would be accessed as `http://your.domain:8123/local/audio.mp3`.

<div class='note'>

  If you've had to create the `www/` folder for the first time, you'll need to restart Home Assistant.

</div>

<div class='note warning'>

  Files served from the `www`/`local` folder, aren't protected by the Home Assistant authentication. Files stored in this folder, if the URL is known, can be accessed by anybody without authentication.

</div>

## Binary Sensor

The HTTP binary sensor is dynamically created with the first request that is made to its URL. You don't have to define it in the configuration first.

The sensor will then exist as long as Home Assistant is running. After a restart of Home Assistant the sensor will be gone until it is triggered again.

The URL for a binary sensor looks like the example below:

```bash
http://IP_ADDRESS:8123/api/states/binary_sensor.DEVICE_NAME
```

<div class='note'>
You should choose a unique device name (DEVICE_NAME) to avoid clashes with other devices.
</div>

The JSON payload must contain the new state and can have a friendly name. The friendly name is used in the frontend to name the sensor.

```json
{"state": "on", "attributes": {"friendly_name": "Radio"}}
```

For a quick test `curl` can be useful to "simulate" a device.

```bash
$ curl -X POST -H "Authorization: Bearer LONG_LIVED_ACCESS_TOKEN" \
    -H "Content-Type: application/json" \
    -d '{"state": "off", "attributes": {"friendly_name": "Radio"}}' \
    http://localhost:8123/api/states/binary_sensor.radio
```

To check if the sensor is working, use again `curl` to retrieve the [current state](/developers/rest_api/#get-apistatesltentity_id).

```bash
$ curl -X GET -H "Authorization: Bearer LONG_LIVED_ACCESS_TOKEN" \
       -H "Content-Type: application/json" \
       http://localhost:8123/api/states/binary_sensor.radio
{
    "attributes": {
        "friendly_name": "Radio"
    },
    "entity_id": "binary_sensor.radio",
    "last_changed": "16:45:51 05-02-2016",
    "last_updated": "16:45:51 05-02-2016",
    "state": "off"
}
```

### Examples

In this section you'll find some real-life examples of how to use this sensor, besides `curl`, which was shown earlier.

#### Using Python request module

As already shown on the [API](/developers/rest_api/) page, it's very simple to use Python and the [Requests](https://requests.kennethreitz.org/en/latest/) module for the interaction with Home Assistant.

```python
response = requests.post(
    "http://localhost:8123/api/states/binary_sensor.radio",
    headers={
        "Authorization": "Bearer LONG_LIVED_ACCESS_TOKEN",
        "content-type": "application/json",
    },
    data=json.dumps({"state": "on", "attributes": {"friendly_name": "Radio"}}),
)
print(response.text)
```

#### Using `httpie`

[`httpie`](https://github.com/jkbrzt/httpie) is a user-friendly CLI HTTP client.

```bash
$ http -v POST http://localhost:8123/api/states/binary_sensor.radio \
      'Authorization:Bearer LONG_LIVED_ACCESS_TOKEN' content-type:application/json state=off \
      attributes:='{"friendly_name": "Radio"}'
```

## Sensor

The HTTP sensor is dynamically created with the first request that is made to its URL. You don't have to define it in the configuration first.

The sensor will then exist as long as Home Assistant is running. After a restart of Home Assistant the sensor will be gone until it is triggered again.

The URL for a sensor looks like the example below:

```bash
http://IP_ADDRESS:8123/api/states/sensor.DEVICE_NAME
```

<div class='note'>
You should choose a unique device name (DEVICE_NAME) to avoid clashes with other devices.
</div>

 The JSON payload must contain the new state and should include the unit of measurement and a friendly name. The friendly name is used in the frontend to name the sensor.

```json
{"state": "20", "attributes": {"unit_of_measurement": "°C", "friendly_name": "Bathroom Temperature"}}
```

For a quick test, `curl` can be useful to "simulate" a device.

```bash
$ curl -X POST -H "Authorization: Bearer LONG_LIVED_ACCESS_TOKEN" \
       -H "Content-Type: application/json" \
       -d '{"state": "20", "attributes": {"unit_of_measurement": "°C", "friendly_name": "Bathroom Temp"}}' \
       http://localhost:8123/api/states/sensor.bathroom_temperature
```

You can then use `curl` again to retrieve the [current sensor state](/developers/rest_api/#get-apistatesltentity_id) and verify the sensor is working.

```bash
$ curl -X GET -H "Authorization: Bearer LONG_LIVED_ACCESS_TOKEN" \
       -H "Content-Type: application/json" \
       http://localhost:8123/api/states/sensor.bathroom_temperature
{
    "attributes": {
        "friendly_name": "Bathroom Temp",
        "unit_of_measurement": "\u00b0C"
    },
    "entity_id": "sensor.bathroom_temperature",
    "last_changed": "09:46:17 06-02-2016",
    "last_updated": "09:48:46 06-02-2016",
    "state": "20"
}
```

For more examples please visit the [HTTP Binary Sensor](#examples) page.
