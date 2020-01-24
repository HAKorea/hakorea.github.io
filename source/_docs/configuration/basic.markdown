---
title: "Setup basic information"
description: "Setting up the basic info of Home Assistant."
redirect_from: /getting-started/basic/
---

As part of the default onboarding process, Home Assistant can detect your location from IP address geolocation. Home Assistant will automatically select a temperature unit and time zone based on this location. You may adjust this during onboarding, or afterwards at Configuration -> General. 

If you prefer YAML, you can add the following information to your `configuration.yaml`:

```yaml
homeassistant:
  latitude: 32.87336
  longitude: 117.22743
  elevation: 430
  unit_system: metric
  time_zone: America/Los_Angeles
  name: Home
  whitelist_external_dirs:
    - /usr/var/dumping-ground
    - /tmp
```

{% configuration %}
latitude:
  description: Latitude of your location required to calculate the time the sun rises and sets.
  required: false
  type: float
longitude:
  description: Longitude of your location required to calculate the time the sun rises and sets.
  required: false
  type: float
elevation:
  description: Altitude above sea level in meters. Impacts weather/sunrise data.
  required: false
  type: integer
unit_system:
  description: "`metric` for Metric, `imperial` for Imperial."
  required: false
  type: string
time_zone:
  description: "Pick your time zone from the column **TZ** of [Wikipedia's list of tz database time zones](http://en.wikipedia.org/wiki/List_of_tz_database_time_zones)"
  required: false
  type: string
name:
  description: Name of the location where Home Assistant is running.
  required: false
  type: string
customize:
  description: "[Customize](/docs/configuration/customizing-devices/) entities."
  required: false
  type: string
customize_domain:
  description: "[Customize](/docs/configuration/customizing-devices/) all entities in a domain."
  required: false
  type: string
customize_glob:
  description: "[Customize](/docs/configuration/customizing-devices/) entities matching a pattern."
  required: false
  type: string
whitelist_external_dirs:
  description: List of folders that can be used as sources for sending files.
  required: false
  type: list
{% endconfiguration %}

### Reload Core Service

Home Assistant offers a service to reload the core configuration while Home Assistant is running called `homeassistant.reload_core_config`. This allows you to change any of the above sections and see it being applied without having to restart Home Assistant. To call this service, go to the "Service" tab under Developer Tools, select the `homeassistant.reload_core_config` service and click the "CALL SERVICE" button. Alternatively, you can press the "Reload Location & Customizations" button under Configuration > Server Control.
