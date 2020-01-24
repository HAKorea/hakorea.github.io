---
title: Ubiquiti UniFi LED
description: Instructions on how to configure the UniFi LED integration with UniFi LED Controller by Ubiquiti.
logo: ubiquiti.png
ha_category:
  - Light
ha_release: 0.102
ha_iot_class: Local Polling
ha_codeowners:
  - '@florisvdk'
---

[UniFi LED](https://unifi-led.ui.com/) by [Ubiquiti Networks, inc.](https://www.ubnt.com/) is a system of controller managed led light panels and dimmers.

There is currently support for the following device type within Home Assistant:

- [Light](#light)

## Configuration

```yaml
# Example configuration.yaml entry
light:
  - platform: unifiled
    host: IP_ADDRESS
    username: USERNAME
    password: PASSWORD
```

{% configuration %}
host:
  description: Ip address or hostname used to connect to the Unifi LED controller.
  type: string
  required: true
  default: None
port:
  description: Port used to connect to the Unifi LED controller.
  type: string
  required: false
  default: 20443
username:
  description: Username used to log into the Unifi LED controller.
  type: string
  required: true
  default: None
password:
  description: Password used to log into the Unifi LED controller.
  type: string
  required: true
  default: None
{% endconfiguration %}

## Light

The light panels output state and brightness are synchronized with Home Assistant.
