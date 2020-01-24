---
title: TP-Link Kasa Smart
description: Instructions on integrating TP-Link Smart Home Devices to Home Assistant.
logo: tp-link.png
ha_category:
  - Hub
  - Switch
  - Light
ha_release: 0.89
ha_iot_class: Local Polling
ha_config_flow: true
ha_codeowners:
  - '@rytilahti'
---

The `tplink` integration allows you to control your [TP-Link Smart Home Devices](https://www.tp-link.com/kasa-smart/) such as smart plugs and smart bulbs.

There is currently support for the following device types within Home Assistant:

- **Light**
- **Switch**

In order to activate the support, you will have to enable the integration inside the config panel.
The supported devices in your network are automatically discovered, but if you want to control devices residing in other networks you will need to configure them manually as shown below.

## Supported Devices

This integration supports devices that are controllable with the [KASA app](https://www.tp-link.com/us/kasa-smart/kasa.html).
The following devices are known to work with this component.

### Plugs

- HS100
- HS103
- HS105
- HS110 (The only device capable or reporting energy usage data to template sensors)

### Strip (Multi-Plug)

- HS107 (indoor 2-outlet)
- HS300 (powerstrip 6-outlet)
- KP303 (powerstrip 3-outlet)
- KP400 (outdoor 2-outlet)
- KP200 (indoor 2-outlet)

### Wall Switches

- HS200
- HS210
- HS220 (acts as a light)

### Bulbs

- LB100
- LB110
- LB120
- LB130
- LB230
- KL110
- KL120
- KL130

## Configuration

```yaml
# Example configuration.yaml
tplink:
```

{% configuration %}
discovery:
  description: Whether to do automatic discovery of devices.
  required: false
  type: boolean
  default: true
light:
  description: List of light devices.
  required: false
  type: list
  keys:
    host:
      description: Hostname or IP address of the device.
      required: true
      type: string
strip:
  description: List of multi-outlet on/off switch devices.
  required: false
  type: list
  keys:
    host:
      description: Hostname or IP address of the device.
      required: true
      type: string
switch:
  description: List of on/off switch devices.
  required: false
  type: list
  keys:
    host:
      description: Hostname or IP address of the device.
      required: true
      type: string
dimmer:
  description: List of dimmable switch devices.
  required: false
  type: list
  keys:
    host:
      description: Hostname or IP address of the device.
      required: true
      type: string
{% endconfiguration %}

## Manual configuration example

```yaml
# Example configuration.yaml entry with manually specified addresses
tplink:
  discovery: false
  light:
    - host: 192.168.200.1
    - host: 192.168.200.2
  switch:
    - host: 192.168.200.3
    - host: 192.168.200.4
  dimmer:
    - host: 192.168.200.5
    - host: 192.168.200.6
  strip:
    - host: 192.168.200.7
    - host: 192.168.200.8
```

## Extracting Energy Sensor data

In order to get the power consumption readings from a TP-Link HS110 device, you'll have to create a [template sensor](/integrations/switch.template/).
In the example below, change all of the `my_tp_switch`'s to match your device's entity ID.

{% raw %}
```yaml
sensor:
  - platform: template
    sensors:
      my_tp_switch_amps:
        friendly_name_template: "{{ states.switch.my_tp_switch.name}} Current"
        value_template: '{{ states.switch.my_tp_switch.attributes["current_a"] | float }}'
        unit_of_measurement: 'A'
      my_tp_switch_watts:
        friendly_name_template: "{{ states.switch.my_tp_switch.name}} Current Consumption"
        value_template: '{{ states.switch.my_tp_switch.attributes["current_power_w"] | float }}'
        unit_of_measurement: 'W'
      my_tp_switch_total_kwh:
        friendly_name_template: "{{ states.switch.my_tp_switch.name}} Total Consumption"
        value_template: '{{ states.switch.my_tp_switch.attributes["total_energy_kwh"] | float }}'
        unit_of_measurement: 'kWh'
      my_tp_switch_volts:
        friendly_name_template: "{{ states.switch.my_tp_switch.name}} Voltage"
        value_template: '{{ states.switch.my_tp_switch.attributes["voltage"] | float }}'
        unit_of_measurement: 'V'
      my_tp_switch_today_kwh:
        friendly_name_template: "{{ states.switch.my_tp_switch.name}} Today's Consumption"
        value_template: '{{ states.switch.my_tp_switch.attributes["today_energy_kwh"] | float }}'
        unit_of_measurement: 'kWh'
```
{% endraw %}
