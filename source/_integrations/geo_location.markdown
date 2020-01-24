---
title: Geolocation
description: Instructions on how to integrate geolocation aware platforms into Home Assistant.
logo: geo_location.png
ha_release: 0.78
---

Geolocation aware entities are typically related to events in the real world in the vicinity of Home Assistant's location, like for example weather events, bush fires or earthquakes.

Entities can have associated geolocation coordinates (latitude and longitude) so that they are displayed on the map. The distance from the entity's coordinates to Home Assistant's location can be used for filtering.

## Geolocation trigger

The [Geolocation trigger](/docs/automation/trigger/#geolocation-trigger) can be used in automations triggered by Geolocation entities appearing in or disappearing from zones. The following value must be used as `source` of the trigger depending on which platform is managing the entities:

| Platform                                          | Source                        |
|---------------------------------------------------|-------------------------------|
| GeoJSON Events                                    | `geo_json_events`               |
| GeoNet New Zealand Quakes                         | `geonetnz_quakes`               |
| IGN Sismología                                    | `ign_sismologia`                |
| NSW Rural Fire Service Incidents                  | `nsw_rural_fire_service_feed`   |
| Queensland Bushfire Alert                         | `qld_bushfire`                  |
| U.S. Geological Survey Earthquake Hazards Program | `usgs_earthquakes_feed`         |
| The World Wide Lightning Location Network         | `wwlln`                         |

Conditions can be used to further filter entities, for example by inspecting their state attributes.

## Geolocation notification example

The following example automation creates a notification on the screen when a fire classified as 'Bush Fire' is reported within a predefined bush fire alert zone:

{% raw %}
```yaml
geo_location:
  - platform: nsw_rural_fire_service_feed
    categories:
      - 'Emergency Warning'
      - 'Watch and Act'
      - 'Advice'

zone:
  - name: Bush Fire Alert Zone
    latitude: -36.666667
    longitude: 149.833333
    radius: 15000
    passive: true

automation:
  - alias: 'Bush Fire Alert'
    trigger:
      platform: geo_location
      source: nsw_rural_fire_service_feed
      zone: zone.bush_fire_alert_zone
      event: enter
    condition:
      condition: template
      value_template: "{{ trigger.to_state.attributes.type == 'Bush Fire' }}"
    action:
      - service: persistent_notification.create
        data_template:
          message: "{{ trigger.to_state.name }} - {{ trigger.to_state.attributes.status }}"
          title: "Bush Fire Alert"
```
{% endraw %}
