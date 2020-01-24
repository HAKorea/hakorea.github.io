---
title: LiteJet
description: Instructions on how to setup the LiteJet hub within Home Assistant.
logo: centralite.svg
ha_category:
  - Light
  - Scene
  - Switch
ha_iot_class: Local Push
ha_release: 0.32
---

LiteJet is a centralized lighting system that predates most home automation technology. All lights and wall switches are wired to a central panel. This central panel has a serial port interface that allows a computer to control the system via LiteJet's third party protocol.

Home Assistant integrates the LiteJet 3rd party protocol and allows you to get the status and control the connected lights.

After connecting the LiteJet's RS232-2 port to your computer, add the following to your `configuration.yaml`:

```yaml
litejet:
  port: /dev/serial/by-id/THE-PATH-OF-YOUR-SERIAL-PORT
```

Your LiteJet MCP should be configured for 19.2 K baud, 8 data bits, 1 stop bit, no parity, and to transmit a 'CR' after each response. These settings can be configured using the [LiteJet programming software](https://www.centralite.com/helpdesk/knowledgebase.php?article=735).

You can also configure the Home Assistant to ignore lights, scenes, and switches via their name. This is highly recommended since LiteJet has a fixed number of each of these and with most systems many will be unused.

{% configuration %}
port:
  description: The path to the serial port connected to the LiteJet.
  required: true
  type: string
exclude_names:
  description: A list of light or switch names that should be ignored.
  required: false
  type: [list, string]
include_switches:
  description: Cause entities to be created for all the LiteJet switches. This can be useful when debugging your lighting as you can press/release switches remotely.
  required: false
  default: false
  type: boolean
{% endconfiguration %}

```yaml
litejet:
  exclude_names:
  - 'Button #'
  - 'Scene #'
  - 'Timed Scene #'
  - 'Timed Scene#'
  - 'LV Rel #'
  - 'Fan #'
```

### Trigger

LiteJet switches can be used as triggers too to allow those buttons to behave differently based on hold time. For example, automation can distinguish quick tap versus long hold.

- **platform** (*Required*): Must be 'litejet'.
- **number** (*Required*): The switch number to be monitored.
- **held_more_than** (*Optional*): The minimum time the switch must be held before the trigger can activate.
- **held_less_than** (*Optional*): The maximum time the switch can be held for the trigger to activate.

The trigger will activate at the earliest moment both `held_more_than` and `held_less_than` are known to be satisfied. If neither are specified, the trigger activates the moment the switch is pressed. If only `held_more_than` is specified, the trigger will activate the moment the switch has been held down at least that time. If `held_less_than` specified, the trigger can only activate when the switch is released.

```yaml
automation:
- trigger:
    platform: litejet
    number: 55
    held_more_than:
      milliseconds: 1000
    held_less_than:
      milliseconds: 2000
```
