---
title: "Template Lock"
description: "Instructions on how to integrate Template Locks into Home Assistant."
ha_category:
  - Lock
ha_release: 0.81
ha_iot_class: Local Push
logo: home-assistant.png
ha_quality_scale: internal
---

The `template` platform creates locks that combines components.

For example, if you have a garage door with a toggle switch that operates the motor and a sensor that allows you know whether the door is open or closed, you can combine these into a lock that knows whether the garage door is open or closed.

This can simplify the GUI and make it easier to write automations. You can mark the integrations you have combined as `hidden` so they don't appear themselves.

In optimistic mode, the lock will immediately change state after every command. Otherwise, the lock will wait for state confirmation from the template. Try to enable it, if experiencing incorrect lock operation.

## Configuration

To enable Template Locks in your installation, add the following to your `configuration.yaml` file:

{% raw %}

```yaml
# Example configuration.yaml entry
lock:
  - platform: template
    name: Garage door
    value_template: "{{ is_state('sensor.door', 'on') }}"
    lock:
      service: switch.turn_on
      data:
        entity_id: switch.door
    unlock:
      service: switch.turn_off
      data:
        entity_id: switch.door
```

{% endraw %}

{% configuration %}
  name:
    description: Name to use in the frontend.
    required: false
    type: string
    default: Template Lock
  value_template:
    description: Defines a template to set the state of the lock.
    required: true
    type: template
  availability_template:
    description: Defines a template to get the `available` state of the component. If the template returns `true`, the device is `available`. If the template returns any other value, the device will be `unavailable`. If `availability_template` is not configured, the component will always be `available`.
    required: false
    type: template
    default: true
 lock:
    description: Defines an action to run when the lock is locked.
    required: true
    type: action
  unlock:
    description: Defines an action to run when the lock is unlocked.
    required: true
    type: action
  optimistic:
    description: Flag that defines if lock works in optimistic mode.
    required: false
    type: boolean
    default: false
{% endconfiguration %}

## Considerations

If you are using the state of a platform that takes extra time to load, the Template Lock may get an `unknown` state during startup. This results in error messages in your log file until that platform has completed loading. If you use `is_state()` function in your template, you can avoid this situation. For example, you would replace {% raw %}`{{ is_state('switch.source', 'on') }}`{% endraw %} with this equivalent that returns `true`/`false` and never gives an unknown result: {% raw %}`{{ is_state('switch.source', 'on') }}`{% endraw %}

## Examples

In this section, you find some real-life examples of how to use this lock.

### Lock from Switch

This example shows a lock that copies data from a switch.

{% raw %}

```yaml
lock:
  - platform: template
    name: Garage Door
    value_template: "{{ is_state('switch.source', 'on') }}"
    lock:
      service: switch.turn_on
      data:
        entity_id: switch.source
    unlock:
      service: switch.turn_off
      data:
        entity_id: switch.source
```

{% endraw %}

### Optimistic Mode

This example shows a lock in optimistic mode. This lock will immediately change state after command and will not wait for state update from the sensor.

{% raw %}

```yaml
lock:
  - platform: template
    name: Garage Door
    value_template: "{{ is_state('sensor.skylight.state', 'on') }}"
    optimistic: true
    lock:
      service: switch.turn_on
      data:
        entity_id: switch.source
    unlock:
      service: switch.turn_off
      data:
        entity_id: switch.source
```

{% endraw %}

### Sensor and Two Switches

This example shows a lock that takes its state from a sensor, and uses two momentary switches to control a device.

{% raw %}

```yaml
lock:
  - platform: template
    name: Garage Door
    value_template: "{{ is_state('sensor.skylight.state', 'on') }}"
    lock:
      service: switch.turn_on
      data:
        entity_id: switch.skylight_open
    unlock:
      service: switch.turn_on
      data:
        entity_id: switch.skylight_close
```

{% endraw %}
