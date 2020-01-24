---
title: Vanderbilt SPC
description: Instructions on how to setup Vanderbilt SPC devices within Home Assistant.
ha_category:
  - Hub
  - Alarm
  - Binary Sensor
ha_release: 0.47
logo: vanderbilt_spc.png
ha_iot_class: Local Push
---

Home Assistant has support to integrate your [Vanderbilt SPC](https://www.spcsupportinfo.com/SPCConnectPro/) alarm panel and any connected motion, door and smoke sensors.

Integration with SPC is done through a third-party API gateway called [SPC Web Gateway](https://www.lundix.se/smarta-losningar/) which must be installed and configured somewhere on your network.

There is currently support for the following device types within Home Assistant:

- [Alarm](#alarm)
- [Binary Sensor](#binary-sensor)

Home Assistant needs to know where to find the SPC Web Gateway API endpoints, to configure this add the following section to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
spc:
  api_url: API_URL
  ws_url: WS_URL
```

{% configuration %}
api_url:
  description: URL of the SPC Web Gateway command REST API, e.g., `http://<ip>:8088`.
  required: true
  type: string
ws_url:
  description: URL of the SPC Web Gateway websocket, e.g., `ws://<ip>:8088/ws/spc`.
  required: true
  type: string
{% endconfiguration %}


## Alarm

The `spc` alarm control panel platform allows you to control your [Vanderbilt SPC](https://www.spcsupportinfo.com/SPCConnectPro/) alarms.

The `changed_by` attribute enables one to be able to take different actions depending on who armed/disarmed the alarm in [automation](/getting-started/automation/).

```yaml
automation:
  - alias: Alarm status changed
    trigger:
      - platform: state
        entity_id: alarm_control_panel.alarm_1
    action:
      - service: notify.notify
        data_template:
          message: >
            {% raw %}Alarm changed from {{ trigger.from_state.state }}
            to {{ trigger.to_state.state }}
            by {{ trigger.to_state.attributes.changed_by }}{% endraw %}
```

## Binary Sensor

The `spc` platform allows you to get data from your [Vanderbilt SPC](https://www.spcsupportinfo.com/SPCConnectPro/) binary sensors from within Home Assistant.

Check the [type/class](/integrations/binary_sensor/) list for a possible visualization of your zone. Currently motion, smoke and door sensors are supported.
