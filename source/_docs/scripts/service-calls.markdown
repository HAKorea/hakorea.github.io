---
title: "Service Calls"
description: "Instructions on how to call services in Home Assistant."
redirect_from: /getting-started/scripts-service-calls/
---

Various integrations allow calling services when a certain event occurs. The most common one is calling a service when an automation trigger happens. But a service can also be called from a script or via the Amazon Echo.

The configuration options to call a config are the same between all integrations and are described on this page.

Examples on this page will be given as part of an automation integration configuration but different approaches can be used for other integrations too.

<div class='note'>
Use the "Services" tab under Developer Tools to discover available services.
</div>

### The basics

Call the service `homeassistant.turn_on` on the entity `group.living_room`. This will turn all members of `group.living_room` on. You can also use `entity_id: all` and it will turn on all possible entities.

```yaml
service: homeassistant.turn_on
entity_id: group.living_room
```

### Passing data to the service call

You can also specify other parameters beside the entity to target. For example, the light turn on service allows specifying the brightness.

```yaml
service: light.turn_on
entity_id: group.living_room
data:
  brightness: 120
  rgb_color: [255, 0, 0]
```

A full list of the parameters for a service can be found on the documentation page of each component, in the same way as it's done for the `light.turn_on` [service](/integrations/light/#service-lightturn_on).

### Use templates to decide which service to call

You can use [templating] support to dynamically choose which service to call. For example, you can call a certain service based on if a light is on.

```yaml
service_template: >
  {% raw %}{% if states('sensor.temperature') | float > 15 %}
    switch.turn_on
  {% else %}
    switch.turn_off
  {% endif %}{% endraw %}
entity_id: switch.ac
```

### Using the Services Developer Tool

You can use the Services Developer Tool to test data to pass in a service call.
For example, you may test turning on or off a 'group' (See [groups] for more info)

To turn a group on or off, pass the following info:
- Domain: `homeassistant`
- Service: `turn_on`
- Service Data: `{ "entity_id": "group.kitchen" }`

### Use templates to determine the attributes

Templates can also be used for the data that you pass to the service call.

```yaml
service: thermostat.set_temperature
data_template:
  entity_id: >
    {% raw %}{% if is_state('device_tracker.paulus', 'home') %}
      thermostat.upstairs
    {% else %}
      thermostat.downstairs
    {% endif %}{% endraw %}
  temperature: {% raw %}{{ 22 - distance(states.device_tracker.paulus) }}{% endraw %}
```

It is even possible to use `data` and `data_template` concurrently but be aware that `data_template` will overwrite attributes that are provided in both.

```yaml
service: thermostat.set_temperature
data:
  entity_id: thermostat.upstairs
data_template:
  temperature: {% raw %}{{ 22 - distance(states.device_tracker.paulus) }}{% endraw %}
```

### `homeassistant` services

There are four `homeassistant` services that aren't tied to any single domain, these are:

* `homeassistant.turn_on` - Turns on an entity (that supports being turned on), for example an `automation`, `switch`, etc
* `homeassistant.turn_off` - Turns off an entity (that supports being turned off), for example an `automation`, `switch`, etc
* `homeassistant.toggle` - Turns off an entity that is on, or turns on an entity that is off (that supports being turned on and off)
* `homeassistant.update_entity` - Request the update of an entity, rather than waiting for the next scheduled update, for example [google travel time] sensor, a [template sensor], or a [light]

Complete service details and examples can be found on the [Home Assistant integration][homeassistant-integration-services] page.

[templating]: /topics/templating/
[google travel time]: /integrations/google_travel_time/
[template sensor]: /integrations/template/
[light]: /integrations/light/
[homeassistant-integration-services]: /integrations/homeassistant#services
