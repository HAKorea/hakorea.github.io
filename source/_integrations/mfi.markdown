---
title: Ubiquiti mFi mPort
description: Instructions on how to integrate mFi within Home Assistant.
logo: ubiquiti.png
ha_category:
  - Network
  - Sensor
  - Switch
ha_iot_class: Local Polling
ha_release: 0.13
---

There is currently support for the following device types within Home Assistant:

- [Sensor](#sensor)
- [Switch](#switch)

## Sensor

The `mfi` sensor platform to allow you to monitor [mFi mPort interface and sensors](https://www.ubnt.com/mfi/mport/).

To add this platform to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: mfi
    host: IP_ADDRESS_OF_SENSOR
    username: YOUR_USERNAME
    password: YOUR_PASSWORD
```

{% configuration %}
host:
  description: The IP address or hostname of your mFi controller.
  required: true
  type: string
port:
  description: The port of your mFi controller.
  required: false
  default: 6080 (6443 for TLS)
  type: integer
username:
  description: The mFi admin username.
  required: true
  type: string
password:
  description: The mFi admin user's password.
  required: true
  type: string
ssl:
  description: If `true`, use SSL/TLS to contact the mFi controller.
  required: false
  default: true
  type: boolean
verify_ssl:
  description: Set this to `false` if your mFi controller has a self-signed certificate.
  required: false
  default: true
  type: boolean
{% endconfiguration %}

## Switch

The `mfi` switch platform to allow you to control [mFi Controllable Power Outlets](https://www.ubnt.com/mfi/mpower/).

To add this platform to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
switch:
  - platform: mfi
    host: IP_ADDRESS_OF_SWITCH
    username: YOUR_USERNAME
    password: YOUR_PASSWORD
```

{% configuration %}
host:
  description: The IP address or hostname of your mFi controller.
  required: true
  type: string
port:
  description: The port of your mFi controller.
  required: false
  default: 6443
  type: integer
username:
  description: The mFi admin username.
  required: true
  type: string
password:
  description: The mFi admin password.
  required: true
  type: string
ssl:
  description: If `true`, use SSL/TLS to contact the mFi controller.
  required: false
  default: true
  type: boolean
verify_ssl:
  description: Set this to `false` if your mFi controller has a self-signed certificate.
  required: false
  default: true
  type: boolean
{% endconfiguration %}
