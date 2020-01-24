---
title: Input Boolean
description: Instructions on how to integrate the Input Boolean integration into Home Assistant.
logo: home-assistant.png
ha_category:
  - Automation
ha_release: 0.11
ha_quality_scale: internal
ha_codeowners:
  - '@home-assistant/core'
---

The `input_boolean` integration allows the user to define boolean values that can be controlled via the frontend and can be used within conditions of automation. This can for example be used to disable or enable certain automations.

To enable input booleans in your installation, add the following lines to your `configuration.yaml`:

```yaml
# Example configuration.yaml entry
input_boolean:
  notify_home:
    name: Notify when someone arrives home
    initial: off
    icon: mdi:car
```

{% configuration %}
  input_boolean:
    description: Alias for the input. Multiple entries are allowed.
    required: true
    type: map
    keys:
      name:
        description: Friendly name of the input.
        required: false
        type: string
      initial:
        description: Initial value when Home Assistant starts.
        required: false
        type: boolean
        default: false
      icon:
        description: Icon to display in front of the input element in the frontend.
        required: false
        type: icon
{% endconfiguration %}

### Services

This integration provides the following services to modify the state of the `input_boolean` and a service to reload the
configuration without restarting Home Assistant itself.

| Service | Data | Description |
| ------- | ---- | ----------- |
| `turn_on` | `entity_id(s)`<br>`area_id(s)` | Set the value of specific `input_boolean` entities to `on`
| `turn_off` | `entity_id(s)`<br>`area_id(s)` | Set the value of specific `input_boolean` entities to `off`
| `toggle` | `entity_id(s)`<br>`area_id(s)` | Toggle the value of specific `input_boolean` entities
| `reload` | | Reload `input_boolean` configuration |

### Restore State

If you set a valid value for `initial` this integration will start with state set to that value. Otherwise, it will restore the state it had prior to Home Assistant stopping.

## Automation Examples

Here's an example of an automation using the above `input_boolean`. This action will only occur if the switch is on.

```yaml
automation:
  alias: Arriving home
  trigger:
    platform: state
    entity_id: binary_sensor.motion_garage
    to: 'on'
  condition:
    condition: state
    entity_id: input_boolean.notify_home
    state: 'on'
  action:
    service: notify.pushbullet
    data:
      title: ""
      message: "Honey, I'm home!"
```

You can also set or change the status of an `input_boolean` by using `input_boolean.turn_on`, `input_boolean.turn_off` or `input_boolean.toggle` in your automations.

```yaml
    - service: input_boolean.turn_on
      data:
        entity_id: input_boolean.notify_home
```
