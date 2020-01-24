---
title: Valloxs
description: Instructions on how to integrate Vallox ventilation units into Home Assistant.
logo: vallox.png
ha_category:
  - Fan
  - Sensor
ha_release: 0.96
ha_iot_class: Local Polling
---

The `vallox` integration lets you control any Vallox ventilation unit that is supported by the [vallox_websocket_api](https://github.com/yozik04/vallox_websocket_api) (follow the link for a list of supported units).

The **fan** platform of this integration allows you to either turn on/off the complete unit via the toggle switch or select a ventilation profile through the service `vallox/set_profile`. The four standard Vallox profiles are provided:

- `At Home`
- `Away`
- `Boost`
- `Fireplace`

Also, there is a **sensor** platform that exposes a number of relevant metrics like fan speed, various air temperatures and humidity.

## Configuration

```yaml
vallox:
  host: IP_ADDRESS
```

{% configuration %}
host:
  description: The IP or hostname of the ventilation unit in your home network.
  required: true
  type: string
name:
  description: The name of this device as you want to see it in Home Assistant.
  required: false
  default: Vallox
  type: string
{% endconfiguration %}

## Profile Switching

For convenient switching of ventilation profiles in the GUI, consider using an [input_select](../input_select) hooked to an automation, for example:

{% raw %}
```yaml
input_select:
  ventilation_profile:
    name: Ventilation profile select
    options:
      - "Home"
      - "Away"
      - "Boost"
      - "Fireplace"
    icon: mdi:fan

automation:
  - alias: Set Ventilation Profile
    trigger:
      platform: state
      entity_id: input_select.ventilation_profile
    action:
      service: vallox.set_profile
      data_template:
        profile: "{{ states('input_select.ventilation_profile') }}"
```
{% endraw %}

## Fan Services

### Service `vallox.set_profile`

Set the ventilation profile.

| Service data attribute | Optional | Description                                          |
|------------------------|----------|------------------------------------------------------|
| `profile`              |       no | Allowed values: `Home`, `Away`, `Boost`, `Fireplace` |

### Service `vallox.set_profile_fan_speed_home`

Set the fan speed of the `Home` profile.

| Service data attribute | Optional | Description                                     |
|------------------------|----------|-------------------------------------------------|
| `fan_speed`            |       no | Fan speed in %. `Integer`, between 0 and 100.   |

### Service `vallox.set_profile_fan_speed_away`

Set the fan speed of the `Away` profile.

| Service data attribute | Optional | Description                                     |
|------------------------|----------|-------------------------------------------------|
| `fan_speed`            |       no | Fan speed in %. `Integer`, between 0 and 100.   |

### Service `vallox.set_profile_fan_speed_boost`

Set the fan speed of the `Boost` profile.

| Service data attribute | Optional | Description                                     |
|------------------------|----------|-------------------------------------------------|
| `fan_speed`            |       no | Fan speed in %. `Integer`, between 0 and 100.   |
