---
title: Wireless Sensor Tags
description: Instructions on how to integrate your Wireless Tags sensors within Home Assistant.
logo: wirelesstag.png
ha_category:
  - Hub
  - Binary Sensor
  - Sensor
  - Switch
ha_iot_class: Cloud Polling and Local Push
ha_release: 0.68
---

The `wirelesstag` implementation allows you to integrate your [wirelesstag.net](https://wirelesstag.net/) sensors tags in Home Assistant.

There is currently support for the following device types within Home Assistant:

- [Binary Sensor](#binary-sensor)
- [Sensor](#sensor)
- [Switch](#switch)

## Configuration

To enable tags set up with your [wirelesstag.net](https://wirelesstag.net/) account, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
wirelesstag:
  username: you@example.com
  password: secret
```

{% configuration %}
username:
  description: Username for your wirelesstag.net account.
  required: true
  type: string
password:
  description: Password for your wirelesstag.net account.
  required: true
  type: string
{% endconfiguration %}

<div class='note'>

To enable local push notifications from the Tags Manager, you need to add the IP address of the Tags Manager into `trusted_networks` under Authentication Providers. See the [Authentication Providers](https://www.home-assistant.io/docs/authentication/providers/#trusted-networks) for details.
 
If you are using a version prior to v0.89 you can do the whitelisting under the [HTTP](/integrations/http) integration.
  
Additionally, you need to add at least one [WirelessTag binary sensor](#binary-sensor) in the configuration to start receiving local push notifications.

</div>

<div class='note warning'>

Tags Manager supports local push notifications for `http` schema only. So if your Home Assistant uses `https`, local push notifications are disabled and data is received via cloud polling.

</div>

## Binary Sensor

To enable the binary sensor platform for your tags, set up with your [wirelesstag.net](https://wirelesstag.net/) account. Add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
binary_sensor:
  - platform: wirelesstag
    monitored_conditions:
      - presence
      - door
      - battery
```

{% configuration %}
monitored_conditions:
  description: The conditions types to monitor.
  required: true
  type: list
  keys:
    presence:
      description: On means in range, Off means out of range.
    motion:
      description: On when a movement was detected, Off when clear.
    door:
      description: On when a door is open, Off when the door is closed.
    cold:
      description: On means temperature become too cold, Off means normal.
    heat:
      description: On means hot, Off means normal.
    dry:
      description: On means too dry (humidity), Off means normal.
    wet:
      description: On means too wet (humidity), Off means normal.
    light:
      description: On means light detected, Off means no light.
    moisture:
      description: On means moisture detected (wet), Off means no moisture (dry).
    battery:
      description: On means tag battery is low, Off means normal.
{% endconfiguration %}

## Sensor

To enable the sensor platform for your tags, set up with your [wirelesstag.net](https://wirelesstag.net/) account. Add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: wirelesstag
    monitored_conditions:
      - temperature
      - humidity
```

{% configuration %}
monitored_conditions:
  description: The metrics types to monitor.
  required: true
  type: list
  keys:
    temperature:
      description: Value is in Celsius or Fahrenheit (according to your settings at Tag Manager).
    humidity:
      description: "Humidity level in %."
    moisture:
      description: "Water level/soil moisture in % (applicable for Water Tag only)."
    light:
      description: Brightness in lux (if supported by tag).
{% endconfiguration %}

## Switch

To enable the switch platform for your tags, set up with your [wirelesstag.net](https://wirelesstag.net/) account. Add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
switch:
  - platform: wirelesstag
    monitored_conditions:
      - motion
      - humidity
```

{% configuration %}
monitored_conditions:
  description: The metrics types to control.
  required: true
  type: list
  keys:
    temperature:
      description: Control arm/disarm temperature monitoring.
    humidity:
      description: Control arm/disarm humidity monitoring.
    motion:
      description: Control arm/disarm motion and door open/close events monitoring.
    light:
      description: Control monitoring of light changes.
    moisture:
      description: Control monitoring of water level/soil moisture for water sensor.
{% endconfiguration %}

Arm/Disarm of motion switch is required to receive motion and door binary sensors events.
Others are only needed if you want to receive push notifications from tags on a specific range of changes in temperature, humidity, light or moisture.
