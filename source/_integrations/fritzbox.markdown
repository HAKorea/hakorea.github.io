---
title: AVM FRITZ!Box
description: Instructions on how to integrate the AVM Fritzbox Smart Home components.
logo: avm.png
ha_category:
  - Binary Sensor
  - Climate
  - Sensor
  - Switch
ha_release: 0.68
ha_iot_class: Local Polling
---

The [AVM](https://en.avm.de) Fritzbox integration for Home Assistant allows you to integrate the switch and climate devices.

There is currently support for the following device types within Home Assistant:

- Binary Sensor
- Climate
- [Sensor](#sensor)
- Switch

#### Tested Devices

- [FRITZ!Box 6490 Cable](https://en.avm.de/products/fritzbox/fritzbox-6490-cable/)
- [FRITZ!Box 7590](https://en.avm.de/products/fritzbox/fritzbox-7590/)
- [FRITZ!Box 7430](https://en.avm.de/products/fritzbox/fritzbox-7430/)
- [FRITZ!DECT 200](https://en.avm.de/products/fritzdect/fritzdect-200/)
- [FRITZ!DECT 301](https://en.avm.de/products/fritzdect/fritzdect-301/)
- [Eurotronic Comet DECT](https://eurotronic.org/produkte/elektronische-heizkoerperthermostate/sparmatic-comet/)

## Setup

```yaml
# Example configuration.yaml entry
fritzbox:
  devices:
    - host: fritz.box
      username: YOUR_USERNAME
      password: YOUR_PASSWORD
```

{% configuration %}
devices:
  description: A list of Fritzbox devices.
  required: true
  type: map
  keys:
    host:
      description: The hostname or IP address of the Fritzbox.
      required: true
      type: string
    username:
      description: The username for Smart Home access.
      required: true
      type: string
    password:
      description: The password of the user.
      required: true
      type: string
{% endconfiguration %}

## Switch & Thermostat

To get AVM Fritzbox switch and thermostat follow the instructions above.

### Attributes

The are several attributes that can be useful for automations and templates.

| Attribute | Description |
| --------- | ----------- |
| `device_locked` | The state of the key lock at the device.
| `locked` | The state of the lock for configuring the device via the app or the Fritzbox web interface.
| `low_battery` | The low battery state indication.
| `battery_level` | The battery level (only available since Fritz!OS 7).
| `holiday_mode` | The state of the holiday mode (only available since Fritz!OS 7).
| `summer_mode` | The state of the summer mode (only available since Fritz!OS 7).
| `window_open` | The state of the window open detection (only available since Fritz!OS 7).
| `temperature_unit` |  The unit of the temperature sensor (only available if the device support temperature sensor).
| `temperature` | The current temperature sensor reading (only available if the device supports temperature sensor).
| `total_consumption` | The total power consumption since the beginning of operation (only available if the device supports power meter function).
| `total_consumption_unit` | The unit of the total_consumption (only available if the device supports power meter function).

## Sensor

To get AVM Fritzbox temperature sensor (e.g. FRITZ!DECT Repeater 100) follow the instructions for the [Fritzbox component](#setup).

### Attributes

The are several attributes that can be useful for automations and templates.

| Attribute | Description |
| --------- | ----------- |
| `device_locked` | The state of the key lock at the device.
| `locked` | The state of the lock for configuring the device via the app or the Fritzbox web interface.
| `temperature_unit` |  The unit of the temperature sensor.
| `temperature` | The current temperature sensor reading.
