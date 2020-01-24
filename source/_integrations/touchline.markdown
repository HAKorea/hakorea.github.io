---
title: Roth Touchline
description: Instructions on how to integrate Roth Touchline within Home Assistant.
logo: roth.png
ha_category:
  - Climate
ha_release: 0.61
ha_iot_class: Local Polling
---

The `touchline` climate platform let you control [ROTH Touchline](http://www.roth-nordic.dk/dk/roth-touchline-tradloes-gulvvarmeregulering-1475.htm) floor heating thermostats from Roth.


To set it up, add the following information to your `configuration.yaml` file:

```yaml
climate:
  - platform: touchline
    host: YOUR_IPADDRESS
```

{% configuration %}
host:
  description: The IP address of your controller, e.g., `http://192.168.1.1`.
  required: false
  type: string
{% endconfiguration %}
