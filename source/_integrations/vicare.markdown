---
title: Viessmann ViCare
description: Instructions how to integrate Viessmann heating devices with Home Assistant
logo: viessmann.png
ha_category: Climate
ha_release: 0.99
ha_iot_class: Cloud Polling
ha_codeowners:
  - '@oischinger'
---

The `ViCare` integration lets you control [Viessmann](https://www.viessmann.com) devices via the Viessmann ViCare (REST) API.
Most recent network-connected Viessmann heating devices (e.g., gas boilers) should be supported.

There is currently support for the following device types within Home Assistant:

- [Climate](#climate) (Heating)
- [Water Heater](#water-heater) (Domestic Hot Water)

## Configuration

To set it up, add the following information to your `configuration.yaml` file:

```yaml
vicare:
  username: VICARE_EMAIL
  password: VICARE_PASSWORD
```

{% configuration %}
username:
  description: Your username for the ViCare App
  required: true
  type: string
password:
  description: Your password for the ViCare App
  required: true
  type: string
name:
  description: The friendly_name of the device (will be appended with *Heating* or *Water*)
  required: false
  default: ViCare
  type: string
circuit:
  description: Heating circuit of your heating device if multiple exist 
  required: false
  type: integer
heating_type:
  description: One of `generic`, `gas` or `heatpump`. Specifying the heating_type provides additional attributes specific for the heating system.
  required: false
  type: string
  default: generic
{% endconfiguration %}

Two components will be created: `climate.vicare_heating` and `water_heater.vicare_water` (for domestic hot water).
Unless you specify a `circuit` parameter, it will pick up the first heating circuit of your installation.

## Climate

A note about the current temperature attribute: Viessmann devices with room temperature sensing will show the current room temperature via `current_temperature`. All other devices will show the current supply temperature of the heating circuit.

### Supported services `climate.vicare_heating`

The following services of the [Climate component](/integrations/climate/) are provided by the ViCare integration: `set_temperature`, `set_hvac_mode`, `set_preset_mode` 

#### Service `set_temperature`

Sets the target temperature to the given temperature.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `entity_id` | yes | String or list of strings that point at `entity_id`'s of climate devices to control. To target all entities, use `all` keyword instead of entity_id.
| `temperature` | no | Desired target temperature

Note that `set_temperature` will always affect the current normal temperature or, if a preset is set, the temperature of the preset (i.e., Viessman program like eco or comfort).

#### Service `climate.set_hvac_mode`

Set HVAC mode for the climate device. The following modes are supported:

The `climate.vicare_heating` component has the following mapping of HVAC modes to Viessmann operation modes:

| HVAC mode | Viessmann mode | Description |
| ---------------------- | -------- | ----------- |
| `off` | `ForcedReduced` | Permanently set heating to reduced temperature. Note: This will also deactivate domestic hot water
| `heat` | `ForcedNormal` | Permanently set heating to normal temperature.
| `auto` | `DHWandHeating` | Switches between reduced and normal temperature as by the heating schedule programmed in your device 
 
| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `entity_id` | yes | String or list of strings that point at `entity_id`'s of climate devices to control. To target all entities, use `all` keyword instead of entity_id.
| `hvac_mode` | no | New value of HVAC mode

#### Service `set_preset_mode`

Sets the preset mode. Supported preset modes are *eco* and *comfort*. These are identical to the respective Viessmann programs and are only temporary. Please consult your heating device manual for more information.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `entity_id` | yes | String or list of strings that point at `entity_id`'s of climate devices to control. To target all entities, use `all` keyword instead of entity_id.
| `preset_mode` | no | New value of hold mode.

## Water Heater

It is not possible to turn on/off water heating via the Water Heater component since this would conflict with the operation modes of the heating component. Therefore the operation mode of that component is just available as an attribute and cannot be modified.

### Supported services `water_heater.vicare_water`

The following services of the [Water Heater component](/integrations/water_heater/) are provided by the ViCare integration: `set_temperature`

#### Service `set_temperature`

Sets the target temperature of domestic hot water to the given temperature.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `entity_id` | yes | String or list of strings that point at `entity_id`'s of water heater devices to control. To target all entities, use `all` keyword instead of entity_id.
| `temperature` | no | New target temperature for water heater
