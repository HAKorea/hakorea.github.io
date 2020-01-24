---
title: Sky Hub
description: Instructions on how to integrate Sky Hub routers into Home Assistant.
logo: sky.png
ha_category:
  - Presence Detection
ha_release: 0.37
---

The `sky_hub` platform offers presence detection by looking at connected devices to a [Sky Hub router](https://www.sky.com/shop/broadband-talk/sky-hub/) based router.

To use your Sky Hub device in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
device_tracker:
  - platform: sky_hub
```

{% configuration %}
host:
  description: The IP address of your router.
  required: false
  default: 192.168.1.254
  type: string
{% endconfiguration %}

See the [device tracker integration page](/integrations/device_tracker/) for instructions how to configure the people to be tracked.
