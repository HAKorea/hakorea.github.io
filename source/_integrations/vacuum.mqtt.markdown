---
title: "MQTT Vacuum"
description: "Instructions on how to integrate your MQTT enabled Vacuum within Home Assistant."
logo: mqtt.png
ha_category:
  - Vacuum
ha_release: 0.54
---

The `mqtt` vacuum integration allows you to control your MQTT-enabled vacuum.
There are two possible message schemas - `legacy` and `state`.
New installations should use the `state` schema as `legacy` is deprecated and might be removed someday in the future.
The `state` schema is preferred because the vacuum will then be represented as a `StateVacuumDevice` which is the preferred parent vacuum entity.

This documentation has 3 sections. Config for `legacy` vacuum with examples, config for `state` vacuum with examples and shared section with examples which are the same for both schemas.

## Configuration

To add your MQTT vacuum to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
vacuum:
  - platform: mqtt
```

## Legacy Configuration

Legacy MQTT vacuum configuration section.

{% configuration %}
name:
  description: The name of the vacuum.
  required: false
  type: string
  default: MQTT Vacuum
schema:
  description: The schema to use.
  required: false
  type: string
  default: legacy
supported_features:
  description: List of features that the vacuum supports (possible values are `turn_on`, `turn_off`, `pause`, `stop`, `return_home`, `battery`, `status`, `locate`, `clean_spot`, `fan_speed`, `send_command`)."
  required: false
  type: [string, list]
  default: "`turn_on`, `turn_off`, `stop`, `return_home`, `status`, `battery`, `clean_spot`"
command_topic:
  description: The MQTT topic to publish commands to control the vacuum.
  required: false
  type: string
qos:
  description: The maximum QoS level of the state topic.
  required: false
  type: integer
  default: 0
retain:
  description: If the published message should have the retain flag on or not.
  required: false
  type: boolean
  default: false
payload_turn_on:
  description: The payload to send to the `command_topic` to begin the cleaning cycle.
  required: false
  type: string
  default: turn_on
payload_turn_off:
  description: The payload to send to the `command_topic` to turn the vacuum off.
  required: false
  type: string
  default: turn_off
payload_return_to_base:
  description: The payload to send to the `command_topic` to tell the vacuum to return to base.
  required: false
  type: string
  default: return_to_base
payload_stop:
  description: The payload to send to the `command_topic` to stop the vacuum.
  required: false
  type: string
  default: stop
payload_clean_spot:
  description: The payload to send to the `command_topic` to begin a spot cleaning cycle.
  required: false
  type: string
  default: clean_spot
payload_locate:
  description: The payload to send to the `command_topic` to locate the vacuum (typically plays a song).
  required: false
  type: string
  default: locate
payload_start_pause:
  description: The payload to send to the `command_topic` to start or pause the vacuum.
  required: false
  type: string
  default: start_pause
battery_level_topic:
  description: The MQTT topic subscribed to receive battery level values from the vacuum.
  required: false
  type: string
battery_level_template:
  description: Defines a [template](/topics/templating/) to define the battery level of the vacuum. This is required if `battery_level_topic` is set.
  required: false
  type: string
charging_topic:
  description: The MQTT topic subscribed to receive charging state values from the vacuum.
  required: false
  type: string
charging_template:
  description: Defines a [template](/topics/templating/) to define the charging state of the vacuum. This is required if `charging_topic` is set.
  required: false
  type: string
cleaning_topic:
  description: The MQTT topic subscribed to receive cleaning state values from the vacuum.
  required: false
  type: string
cleaning_template:
  description: Defines a [template](/topics/templating/) to define the cleaning state of the vacuum. This is required if `cleaning_topic` is set.
  required: false
  type: string
docked_topic:
  description: The MQTT topic subscribed to receive docked state values from the vacuum.
  required: false
  type: string
docked_template:
  description: Defines a [template](/topics/templating/) to define the docked state of the vacuum. This is required if `docked_topic` is set.
  required: false
  type: string
error_topic:
  description: The MQTT topic subscribed to receive error messages from the vacuum.
  required: false
  type: string
error_template:
  description: Defines a [template](/topics/templating/) to define potential error messages emitted by the vacuum. This is required if `error_topic` is set.
  required: false
  type: string
fan_speed_topic:
  description: The MQTT topic subscribed to receive fan speed values from the vacuum.
  required: false
  type: string
fan_speed_template:
  description: Defines a [template](/topics/templating/) to define the fan speed of the vacuum. This is required if `fan_speed_topic` is set.
  required: false
  type: string
set_fan_speed_topic:
  description: The MQTT topic to publish commands to control the vacuum's fan speed.
  required: false
  type: string
fan_speed_list:
  description: List of possible fan speeds for the vacuum.
  required: false
  type: [string, list]
send_command_topic:
  description: The MQTT topic to publish custom commands to the vacuum.
  required: false
  type: string
availability_topic:
  description: The MQTT topic subscribed to receive availability (online/offline) updates.
  required: false
  type: string
payload_available:
  description: The payload that represents the available state.
  required: false
  type: string
  default: online
payload_not_available:
  description: The payload that represents the unavailable state.
  required: false
  type: string
  default: offline
json_attributes_topic:
  description: The MQTT topic subscribed to receive a JSON dictionary payload and then set as sensor attributes. Usage example can be found in [MQTT sensor](/integrations/sensor.mqtt/#json-attributes-topic-configuration) documentation.
  required: false
  type: string
json_attributes_template:
  description: "Defines a [template](/docs/configuration/templating/#processing-incoming-data) to extract the JSON dictionary from messages received on the `json_attributes_topic`. Usage example can be found in [MQTT sensor](/integrations/sensor.mqtt/#json-attributes-template-configuration) documentation."
  required: false
  type: template
{% endconfiguration %}

### Legacy configuration example

{% raw %}
```yaml
# Example configuration.yaml entry
vacuum:
  - platform: mqtt
    name: "MQTT Vacuum"
    supported_features:
      - turn_on
      - turn_off
      - pause
      - stop
      - return_home
      - battery
      - status
      - locate
      - clean_spot
      - fan_speed
      - send_command
    command_topic: "vacuum/command"
    battery_level_topic: "vacuum/state"
    battery_level_template: "{{ value_json.battery_level }}"
    charging_topic: "vacuum/state"
    charging_template: "{{ value_json.charging }}"
    cleaning_topic: "vacuum/state"
    cleaning_template: "{{ value_json.cleaning }}"
    docked_topic: "vacuum/state"
    docked_template: "{{ value_json.docked }}"
    error_topic: "vacuum/state"
    error_template: "{{ value_json.error }}"
    fan_speed_topic: "vacuum/state"
    fan_speed_template: "{{ value_json.fan_speed }}"
    set_fan_speed_topic: "vacuum/set_fan_speed"
    fan_speed_list:
      - min
      - medium
      - high
      - max
    send_command_topic: 'vacuum/send_command'
```
{% endraw %}

## Legacy MQTT Protocol

The above configuration for this integration expects an MQTT protocol like the following.

### Legacy Basic Commands

MQTT topic: `vacuum/command`

Possible MQTT payloads:

- `turn_on` - Begin cleaning
- `turn_off` - Turn the Vacuum off
- `return_to_base` - Return to base/dock
- `stop` - Stop the Vacuum
- `clean_spot` - Initialize a spot cleaning cycle
- `locate` - Locate the vacuum (typically by playing a song)
- `start_pause` - Toggle the vacuum between cleaning and stopping

### Status/Sensor Updates

MQTT topic: `vacuum/state`

MQTT payload:

```json
{
    "battery_level": 61,
    "docked": true,
    "cleaning": false,
    "charging": true,
    "fan_speed": "off",
    "error": "Error message"
}
```

## State Configuration

State MQTT vacuum configuration section.

{% configuration %}
name:
  description: The name of the vacuum.
  required: false
  type: string
  default: MQTT Vacuum
schema:
  description: The schema to use.
  required: false
  type: string
  default: legacy
supported_features:
  description: "List of features that the vacuum supports (possible values are `start`, `stop`, `pause`, `return_home`, `battery`, `status`, `locate`, `clean_spot`, `fan_speed`, `send_command`)."
  required: false
  type: [string, list]
  default: "`start`, `stop`, `return_home`, `status`, `battery`, `clean_spot`"
command_topic:
  description: The MQTT topic to publish commands to control the vacuum.
  required: false
  type: string
qos:
  description: The maximum QoS level of the state topic.
  required: false
  type: integer
  default: 0
retain:
  description: If the published message should have the retain flag on or not.
  required: false
  type: boolean
  default: false
payload_start:
  description: "The payload to send to the `command_topic` to begin the cleaning cycle."
  required: false
  type: string
  default: start
payload_stop:
  description: "The payload to send to the `command_topic` to stop cleaning."
  required: false
  type: string
  default: stop
payload_return_to_base:
  description: The payload to send to the `command_topic` to tell the vacuum to return to base.
  required: false
  type: string
  default: return_to_base
payload_clean_spot:
  description: The payload to send to the `command_topic` to begin a spot cleaning cycle.
  required: false
  type: string
  default: clean_spot
payload_locate:
  description: The payload to send to the `command_topic` to locate the vacuum (typically plays a song).
  required: false
  type: string
  default: locate
payload_pause:
  description: The payload to send to the `command_topic` to pause the vacuum.
  required: false
  type: string
  default: pause
state_topic:
  description: The MQTT topic subscribed to receive state messages from the vacuum. State topic is extracting json if no `value_template` is defined.
  required: false
  type: string
value_template:
  description: "Defines a [template](/topics/templating/) to extract possible states from the vacuum."
  required: false
  type: string
set_fan_speed_topic:
  description: The MQTT topic to publish commands to control the vacuum's fan speed.
  required: false
  type: string
fan_speed_list:
  description: List of possible fan speeds for the vacuum.
  required: false
  type: [string, list]
send_command_topic:
  description: The MQTT topic to publish custom commands to the vacuum.
  required: false
  type: string
availability_topic:
  description: The MQTT topic subscribed to receive availability (online/offline) updates.
  required: false
  type: string
payload_available:
  description: The payload that represents the available state.
  required: false
  type: string
  default: online
payload_not_available:
  description: The payload that represents the unavailable state.
  required: false
  type: string
  default: offline
json_attributes_topic:
  description: The MQTT topic subscribed to receive a JSON dictionary payload and then set as sensor attributes. Usage example can be found in [MQTT sensor](/integrations/sensor.mqtt/#json-attributes-topic-configuration) documentation.
  required: false
  type: string
{% endconfiguration %}

### State configuration example

{% raw %}
```yaml
# Example configuration.yaml entry
vacuum:
  - platform: mqtt
    name: "MQTT Vacuum"
    schema: state
    supported_features:
      - start
      - pause
      - stop
      - return_home
      - battery
      - status
      - locate
      - clean_spot
      - fan_speed
      - send_command
    command_topic: "vacuum/command"
    state_topic: "vacuum/state"
    set_fan_speed_topic: "vacuum/set_fan_speed"
    fan_speed_list:
      - min
      - medium
      - high
      - max
    send_command_topic: 'vacuum/send_command'
```
{% endraw %}

## State MQTT Protocol

The above configuration for this integration expects an MQTT protocol like the following.

### State Basic Commands

MQTT topic: `vacuum/command`

Possible MQTT payloads:

- `start` - Start cleaning
- `pause` - Pause cleaning
- `return_to_base` - Return to base/dock
- `stop` - Stop the vacuum.
- `clean_spot` - Initialize a spot cleaning cycle
- `locate` - Locate the vacuum (typically by playing a song)

### Send Custom Command

Vacuum send_command allows three parameters:

- entity_id
- command
- params - optional

If params are not provided it sends command as payload to MQTT send_command topic.
If params are provided service sends json as payload with such structure:

```json
{
  'command': 'command',
  'param1-key': 'param1-value'
}
```

Service trigger example:

```yaml
- alias: Push command based on sensor
    trigger:
      - platform: state
        entity_id: sensor.sensor
    action:
      service: vacuum.send_command
      data:
        entity_id: 'vacuum.vacuum_entity'
        command: 'custom_command'
        params:
          - key: value
```

MQTT topic: `vacuum/send_command`

### Status/Sensor Updates

MQTT topic: `vacuum/state`

MQTT payload:

```json
{
    "battery_level": 61,
    "state": "docked",
    "fan_speed": "off"
}
```

State has to be one of vacuum states supported by Home Assistant:

- cleaning,
- docked,
- paused,
- idle,
- returning,
- error.

## Shared MQTT Protocol

The configuration for this integration expects an MQTT protocol like the following.
These services are identical for both - legacy and state vacuum.

### Set Fan Speed

MQTT topic: `vacuum/set_fan_speed`

Possible MQTT payloads:

- `min` - Minimum fan speed
- `medium` - Medium fan speed
- `high` - High fan speed
- `max` - Max fan speed

### Send Custom Command

Vacuum send_command allows three parameters:

- entity_id
- command
- params - optional

If params are not provided it sends command as payload to MQTT send_command topic.
If params are provided service sends json as payload with such structure:

```json
{
  'command': 'command',
  'param1-key': 'param1-value'
}
```

Service trigger example:

```yaml
- alias: Push command based on sensor
    trigger:
      - platform: state
        entity_id: sensor.sensor
    action:
      service: vacuum.send_command
      data:
        entity_id: 'vacuum.vacuum_entity'
        command: 'custom_command'
        params:
          - key: value
```

MQTT topic: `vacuum/send_command`

### Usage with cloudless Xiaomi vacuums

This integration is supported by the cloud-free Xiaomi Vacuum Webinterface [Valetudo](https://github.com/Hypfer/Valetudo).

### Retrofitting non-wifi vacuums

- Retrofitting your old Roomba with an ESP8266. [This repo](https://github.com/johnboiles/esp-roomba-mqtt) provides MQTT client firmware.
- If you own a non-wifi Neato, you can refer to [this repo](https://github.com/jeroenterheerdt/neato-serial) that uses a Raspberry Pi to retrofit an old Neato.
