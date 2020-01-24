---
title: Leviton Decora Wi-Fi
description: Instructions on how to setup Leviton Decora Smart Wi-Fi switches/dimmers within Home Assistant.
ha_category:
  - Light
ha_iot_class: Cloud Polling
logo: leviton.png
ha_release: 0.51
---

Support for [Leviton Decora Wi-Fi](https://www.leviton.com/en/products/lighting-controls/decora-smart-with-wifi) dimmers/switches via the MyLeviton API.

Supported devices (tested):

- [DW6HD1-BZ](https://www.leviton.com/en/products/dw6hd-1bz) (Decora Smart Wi-Fi 600W Dimmer)
- [DW15S-1BZ](https://www.leviton.com/en/products/dw15s-1bz) (Decora Smart Wi-Fi 15A Switch)

To enable these lights, add the following lines to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
light:
  - platform: decora_wifi
    username: YOUR_USERNAME
    password: YOUR_PASSWORD
```

{% configuration %}
username:
  description: Your "My Leviton" app email address/user name.
  required: true
  type: string
password:
  description: Your "My Leviton" app password.
  required: true
  type: string
{% endconfiguration %}
