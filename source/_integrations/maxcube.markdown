---
title: eQ-3 MAX!
description: Instructions on how to integrate eQ-3 MAX! components with Home Assistant via eQ-3 MAX! Cube.
logo: maxcube.png
ha_category:
  - Climate
  - Binary Sensor
ha_release: '0.40'
ha_iot_class: Local Polling
---

[eQ-3 MAX!](https://www.eq-3.com/products/max.html) integration for Home Assistant allows you to connect eQ-3 MAX! components via the eQ-3 MAX! Cube. The components connects to the eQ-3 MAX! Cube via TCP and automatically makes all supported integrations available in Home Assistant. The name for each device is created by concatenating the MAX! room and device names.

There is currently support for the following device types within Home Assistant:

- Binary Sensor
- Climate

Limitations:

- Configuring weekly schedules is not possible.
- Implementation is based on the reverse engineered [MAX! protocol](https://github.com/Bouni/max-cube-protocol).

Supported Devices:

- MAX! Radiator Thermostat (tested)
- MAX! Radiator Thermostat+
- MAX! Window Sensor (tested)
- MAX! Wall Thermostat (tested)

### One Gateway

A `maxcube` section must be present in the `configuration.yaml` file and contain the following options as required:

```yaml
# Example configuration.yaml entry
maxcube:
  gateways:
    - host: 192.168.0.20
```

### Multiple Gateways

```yaml
# Example configuration.yaml entry
maxcube:
  gateways:
    - host: 192.168.0.20
      port: 62910
    - host: 192.168.0.21
      port: 62910
```

{% configuration %}
  host:
    description: The IP address of the eQ-3 MAX! Cube to use.
    required: true
    type: string
  port:
    description: The UDP port number.
    required: false
    type: integer
    default: 62910
  scan_interval:
    description: The update interval in seconds
    required: false
    type: integer
    default: 300
{% endconfiguration %}
