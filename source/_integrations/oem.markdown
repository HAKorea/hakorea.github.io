---
title: OpenEnergyMonitor WiFi Thermostat
description: Instructions on how to integrate an OpenEnergyMonitor thermostat with Home Assistant.
logo: oem.png
ha_category:
  - Climate
ha_release: 0.39
ha_iot_class: Local Polling
---

This platform supports the ESP8266 based "WiFi MQTT Relay / Thermostat" sold by [OpenEnergyMonitor](https://shop.openenergymonitor.com/wifi-mqtt-relay-thermostat/). The underlying [library](https://oemthermostat.readthedocs.io/) only supports this single relay variant of the [original device](https://harizanov.com/2014/12/wifi-iot-3-channel-relay-board-with-mqtt-and-http-api-using-esp8266/).

This platform controls the setpoint of the thermostat in its "manual" mode.

To set it up, add the following information to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
climate oem:
  - platform: oem
    host: 192.168.0.100
```

{% configuration %}
host:
  description: The IP address or hostname of the thermostat.
  required: true
  type: string
port:
  description: The port for the web interface.
  required: false
  default: 80
  type: integer
name:
  description: The name to use for the frontend.
  required: false
  default: Thermostat
  type: string
username:
  description: Username for the web interface if set.
  required: inclusive
  type: string
password:
  description: Password for the web interface if set.
  required: inclusive
  type: string
{% endconfiguration %}
