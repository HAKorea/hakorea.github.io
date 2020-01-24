---
title: "Template Alarm Control Panel"
description: "Instructions on how to integrate Template Alarm Control Panels into Home Assistant."
ha_category: 
  - Alarm
ha_release: 0.105
ha_iot_class: "Local Push"
logo: home-assistant.png
ha_qa_scale: internal
---

The `template` integrations creates alarm control panels that combine integrations or adds pre-processing logic to actions.

There are several powerful ways to use this integration, including grouping existing integrations into a simpler integrations, or adding logic that Home Assistant will execute when accessed.

For example, if you want to expose a true alarm panel to Google Home, Alexa, or Homekit - but limit its ability to disarm when there's no one home, you can do that using a template.

Another use case could be grouping a series of sensors and services together to represent various "armed" and "disarmed" states and actions.

This can simplify the GUI and make it easier to write automations.

In optimistic mode, the alarm control panel will immediately change state after every command. Otherwise, the alarm control panel will wait for state confirmation from the template. Try to enable it, if experiencing incorrect operation.

## Configuration

To enable a Template Alarm Control Panel in your installation, add the following to your `configuration.yaml` file:

{% raw %}

```yaml
# Example configuration.yaml entry
alarm_control_panel:
  - platform: template
    name: Safe Alarm Panel
    value_template: "{{ states('alarm_control_panel.real_alarm') }}"
    arm_away:
      service: alarm_control_panel.alarm_arm_away
      data:
        entity_id: alarm_control_panel.real_alarm
        code: !secret alarm_code
    arm_home:
      service: alarm_control_panel.alarm_arm_home
      data:
        entity_id: alarm_control_panel.real_alarm
        code: !secret alarm_code
    disarm:
      - condition: state
        entity_id: device_tracker.paulus
        state: 'home'
      - service: alarm_control_panel.alarm_arm_home
        data:
          entity_id: alarm_control_panel.real_alarm
          code: !secret alarm_code
```

{% endraw %}

{% configuration %}
  name:
    description: Name to use in the frontend.
    required: false
    type: string
    default: Template Alarm Control Panel
  value_template:
    description: "Defines a template to set the state of the alarm panel. Only the states `armed_away`, `armed_home`, `armed_night`, `disarmed`, `triggered` and `unavailable` are used."
    required: false
    type: template
  disarm:
    description: Defines an action to run when the alarm is disarmed.
    required: false
    type: action
  arm_away:
    description: Defines an action to run when the alarm is armed to away mode.
    required: false
    type: action
  arm_home:
    description: Defines an action to run when the alarm is armed to home mode.
    required: false
    type: action
  arm_night:
    description: Defines an action to run when the alarm is armed to night mode.
    required: false
    type: action
{% endconfiguration %}

## Considerations

If you are using the state of a integration that takes extra time to load, the Template Alarm Control Panel may get an `unknown` state during startup. This results in error messages in your log file until that integration has completed loading. If you use `is_state()` function in your template, you can avoid this situation.

For example, you would replace {% raw %}`{{ states.switch.source.state == 'on' }}`{% endraw %} with this equivalent that returns `true`/`false` and never gives an unknown result: {% raw %}`{{ is_state('switch.source', 'on') }}`{% endraw %}
