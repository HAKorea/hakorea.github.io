---
title: Input Number
description: Instructions on how to integrate the Input Number integration into Home Assistant.
logo: home-assistant.png
ha_category:
  - Automation
ha_release: 0.55
ha_quality_scale: internal
ha_codeowners:
  - '@home-assistant/core'
---

The `input_number` integration allows the user to define values that can be controlled via the frontend and can be used within conditions of automation. The frontend can display a slider, or a numeric input box. Changes to the slider or numeric input box generate state events. These state events can be utilized as `automation` triggers as well.

To enable this input number in your installation, add the following lines to your `configuration.yaml`:

```yaml
# Example configuration.yaml entry
input_number:
  slider1:
    name: Slider
    initial: 30
    min: -20
    max: 35
    step: 1
  box1:
    name: Numeric Input Box
    initial: 30
    min: -20
    max: 35
    step: 1
    mode: box
```

{% configuration %}
  input_number:
    description: Alias for the input. Multiple entries are allowed.
    required: true
    type: map
    keys:
      min:
        description: Minimum value.
        required: true
        type: float
      max:
        description: Maximum value.
        required: true
        type: float
      name:
        description: Friendly name of the input.
        required: false
        type: string
      initial:
        description: Initial value when Home Assistant starts.
        required: false
        type: float
        default: The value at shutdown
      step:
        description: Step value for the slider. Smallest value `0.001`.
        required: false
        type: float
        default: 1
      mode:
        description: Can specify `box` or `slider`.
        required: false
        type: string
        default: slider
      unit_of_measurement:
        description: Unit of measurement in which the value of the slider is expressed in.
        required: false
        type: string
      icon:
        description: Icon to display in front of the input element in the frontend.
        required: false
        type: icon
{% endconfiguration %}

### Services

This integration provides the following services to modify the state of the `input_number` and a service to reload the
configuration without restarting Home Assistant itself.

| Service | Data | Description |
| ------- | ---- | ----------- |
| `decrement` | `entity_id(s)`<br>`area_id(s)` | Decrement the value of specific `input_number` entities by `step` 
| `increment` | `entity_id(s)`<br>`area_id(s)` | Increment the value of specific `input_number` entities by `step`
| `reload` | | Reload `input_number` configuration |
| `set_value` | `value`<br>`entity_id(s)`<br>`area_id(s)` | Set the value of specific `input_number` entities

### Restore State

If you set a valid value for `initial` this integration will start with state set to that value. Otherwise, it will restore the state it had prior to Home Assistant stopping.

### Scenes

To set the value of an input_number in a [Scene](/integrations/scene/):

```yaml
# Example configuration.yaml entry
scene:
  - name: Example Scene
    entities:
      input_number.example_number: 13
```

## Automation Examples

Here's an example of `input_number` being used as a trigger in an automation.

{% raw %}
```yaml
# Example configuration.yaml entry using 'input_number' as a trigger in an automation
input_number:
  bedroom_brightness:
    name: Brightness
    initial: 254
    min: 0
    max: 254
    step: 1
automation:
  - alias: Bedroom Light - Adjust Brightness
    trigger:
      platform: state
      entity_id: input_number.bedroom_brightness
    action:
      - service: light.turn_on
        # Note the use of 'data_template:' below rather than the normal 'data:' if you weren't using an input variable
        data_template:
          entity_id: light.bedroom
          brightness: "{{ trigger.to_state.state | int }}"
```
{% endraw %}

Another code example using `input_number`, this time being used in an action in an automation.

{% raw %}
```yaml
# Example configuration.yaml entry using 'input_number' in an action in an automation
input_select:
  scene_bedroom:
    name: Scene
    options:
      - Select
      - Concentrate
      - Energize
      - Reading
      - Relax
      - 'OFF'
    initial: 'Select'
input_number:
  bedroom_brightness:
    name: Brightness
    initial: 254
    min: 0
    max: 254
    step: 1
automation:
  - alias: Bedroom Light - Custom
    trigger:
      platform: state
      entity_id: input_select.scene_bedroom
      to: CUSTOM
    action:
      - service: light.turn_on
        # Again, note the use of 'data_template:' rather than the normal 'data:' if you weren't using an input variable.
        data_template:
          entity_id: light.bedroom
          brightness: "{{ states('input_number.bedroom_brightness') | int }}"
```
{% endraw %}

Example of `input_number` being used in a bidirectional manner, both being set by and controlled by an MQTT action in an automation.

{% raw %}
```yaml
# Example configuration.yaml entry using 'input_number' in an action in an automation
input_number:
  target_temp:
    name: Target Heater Temperature Slider
    min: 1
    max: 30
    step: 1
    unit_of_measurement: step  
    icon: mdi:target

# This automation script runs when a value is received via MQTT on retained topic: setTemperature
# It sets the value slider on the GUI. This slides also had its own automation when the value is changed.
automation:
  - alias: Set temp slider
    trigger:
      platform: mqtt
      topic: 'setTemperature'
    action:
      service: input_number.set_value
      data_template:
        entity_id: input_number.target_temp
        value: "{{ trigger.payload }}"

# This second automation script runs when the target temperature slider is moved.
# It publishes its value to the same MQTT topic it is also subscribed to.
  - alias: Temp slider moved
    trigger:
      platform: state
      entity_id: input_number.target_temp
    action:
      service: mqtt.publish
      data_template:
        topic: 'setTemperature'
        retain: true
        payload: "{{ states('input_number.target_temp') | int }}"
```
{% endraw %}

Here's an example of `input_number` being used as a delay in an automation.

{% raw %}
```yaml
# Example configuration.yaml entry using 'input_number' as a delay in an automation
input_number:
  minutes:
    name: minutes
    icon: mdi:clock-start
    initial: 3
    min: 0
    max: 6
    step: 1
    
  seconds:
    name: seconds
    icon: mdi:clock-start
    initial: 30
    min: 0
    max: 60
    step: 10
    
automation:
 - alias: turn something off after x time after turning it on
   trigger:
     platform: state
     entity_id: switch.something
     to: 'on'
   action:
     - delay: '00:{{ states('input_number.minutes') | int }}:{{ states('input_number.seconds') | int }}'
     - service: switch.turn_off
       entity_id: switch.something
```
{% endraw %}
