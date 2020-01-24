---
title: HomematicIP Cloud
description: Instructions for integrating HomematicIP into Home Assistant.
logo: homematicip_cloud.png
ha_category:
  - Hub
  - Alarm
  - Binary Sensor
  - Climate
  - Cover
  - Light
  - Sensor
  - Switch
ha_iot_class: Cloud Push
ha_release: 0.66
ha_config_flow: true
ha_codeowners:
  - '@SukramJ'
---

The [HomematicIP](https://www.homematic-ip.com/) integration platform is used as an interface to the cloud server. Since there is no official documentation about this API, everything was done via reverse engineering. The [homematicip-rest-api](https://github.com/coreGreenberet/homematicip-rest-api) is used for communicating. Use at your own risk.

There is currently support for the following device types within Home Assistant:

* Alarm
* Binary Sensor
* Climate
* Cover
* Light
* Sensor
* Switch
* Weather

## Setup the integration via the frontend

Menu: *Configuration* -> *Integrations*

Fill the form:

* Your **access point ID** (SGTIN)
* Optional a **name** to identify your access point, this will be used to prefix your device names.
* The **PIN**, mandatory if one is used in the native HomematicIP App.

The authentification token will be generated and stored internally.

## Setup the integration using the configuration files

Generate the authentication token:

`hmip_generate_auth_token.py`

Add the information to your `configuration.yaml` file:

```yaml
homematicip_cloud:
  - accesspoint: IDENTIFIER
    authtoken: AUTHTOKEN
  - name: Location2
    accesspoint: IDENTIFIER2
    authtoken: AUTHTOKEN2
```

{% configuration %}
name:
  required: false
  description: Name to identify your access point, this will be used to prefix your device names.
  type: string
accesspoint:
  required: true
  description: This is the access point ID (SGTIN).
  type: string
authtoken:
  required: true
  description: "Authentication token generated with `generate_auth_token.py`."
  type: string
{% endconfiguration %}

## Adding and removing devices and group via native HomematicIP APP

Devices and groups are instantly removed from Homeassistant when removed in the native HomematicIP APP.
Groups are instantly created in Homeassistant when created in the native HomematicIP APP.
Devices are created with a delay of 30 seconds in Homeassistant when created in the native HomematicIP APP.
Within this delay the device registration should be completed in the App, otherwise the device name will be a default one based on the device type. This can easily be fixed in the Homeassistant entity registry afterwards.

## Implemented and tested devices

* homematicip_cloud.alarm_control_panel
  * Combined Alarm Control Panal with INTERNAL and EXTERNAL Security zones (*HmIP-SecurityZone*)

* homematicip_cloud.binary_sensor
  * Acceleration Sensor (*HMIP-SAM*)
  * Window and door contact (*HmIP-SWDO, -I*)
  * Contact Interface flush-mount – 1 channel (*HmIP-FCI1*)
  * Contact Interface (*HmIP-SCI*)
  * Window Rotary Handle Sensor (*HmIP-SRH*)
  * Smoke sensor and alarm (*HmIP-SWSD*)
  * Motion Detector with Brightness Sensor - indoor (*HmIP-SMI*)
  * Motion Detector with Brightness Sensor - outdoor (*HmIP-SMO*)
  * Presence Sensor – indoor (*HmIP-SPI*)
  * Water Sensor (*HmIP-SWD*)
  * Remote Control - 8 buttons (*HmIP-RC8*) (battery only)
  * Wall-mount Remote Control - 2-button (*HmIP-WRC2*) (battery only)
  * Wall-mount Remote Control - 6-button (*HmIP-WRC6*) (battery only)
  * Key Ring Remote Control - 4 buttons (*HmIP-KRC4*) (battery only)
  * Key Ring Remote Control - alarm  (*HmIP-KRCA*) (battery only)
  * Alarm Siren (*HmIP-ASIR, -B1*) (battery only)
  * Remote Control for brand switches – 2-button (*HmIP-BRC2*) (battery only)
  * Pluggable Power Supply Monitoring (*HmIP-PMFS*)

* homematicip_cloud.climate
  * Climate group (*HmIP-HeatingGroup*)
  * This includes temperature/humidity measures for climate devices of a room delivered by:
    * Wall-mounted thermostat (*HmIP-WTH, WTH-2*)
    * Brand Wall-mounted thermostat (*HmIP-BWTH, BWTH-24*)
    * Radiator thermostat (*HmIP-eTRV,-2,-C*) - should also work with (*HmIP-eTRV-2-UK, -B, -B1*)
    * Temperature and humidity sensor (*HmIP-STH*)
    * Temperature and humidity Sensor with display (*HmIP-STHD*)
  * There is no need to directly support the following devices by Home Assistant, because their integration is done by the required wall thermostats:
    * Floor Heating Actuator – 6x channels, 230V (*HMIP-FAL230-C6*)
    * Floor Heating Actuator – 10x channels, 230V (*HMIP-FAL230-C10*)
    * Floor Heating Actuator – 6x channels, 24V (*HMIP-FAL24-C6*)
    * Floor Heating Actuator – 10x channels, 24V (*HMIP-FAL24-C10*)
    * Floor Heating Actuator – 12x channels, motorised (*HMIP-FALMOT-C12*)

* homematicip_cloud.cover
  * Shutter actuator for brand-mount (*HmIP-BROLL*)
  * Shutter actuator for flush-mount (*HmIP-FROLL*)
  * Blind Actuator for brand switches (*HmIP-BBL*)
  * Blind Actuator for flush-mount (*HmIP-FBL*)
  * Garage door module for Tormatic (*HmIP-MOD_TM*)

* homematicip_cloud.light
  * Switch actuator and meter for brand switches (*HmIP-BSM*)
  * Dimming actuator for brand switches (*HmIP-BDT*)
  * Dimming actuator flush-mount (*HmIP-FDT*)
  * Pluggable Dimmer – trailing edge (*HmIP-PDT*)
  * Switch Actuator for brand switches – with signal lamp (*HmIP-BSL*)

* homematicip_cloud.sensor
  * Cloud Access point duty-cycle (*HmIP-HAP, -B1*)
  * Wall Mounted Thermostat Pro with Display (*HmIP-WTH, WTH2*)
  * Radiator thermostat (*HmIP-eTRV,-2, -C*) - should also work with (*HmIP-eTRV-2-UK, -B, -B1*)
  * Temperature and Humidity Sensor without display - indoor (*HmIP-STH*)
  * Temperature and Humidity Sensor with display - indoor (*HmIP-STHD*)
  * Temperature and Humidity sensor - outdoor (*HmIP-STHO, -A*)
  * Motion Detector with Brightness Sensor - indoor (*HmIP-SMI*)
  * Motion Detector with Brightness Sensor - outdoor (*HmIP-SMO*)
  * Presence Sensor – indoor (*HmIP-SPI*)
  * Light Sensor - outdoor (*HmIP-SLO*)
  * Passage Sensor with Direction Recognition (*HmIP-SPDR*) (delta counter)

* homematicip_cloud.switch
  * Pluggable Switch (*HmIP-PS*)
  * Pluggable Switch and Meter (*HmIP-PSM*) - should also work with (*HmIP-PSM-CH, -IT, -UK, -PE*)
  * Switch Actuator and Meter – flush-mount (*HmIP-FSM, -FSM16*)
  * Open Collector Module Receiver - 8x (*HmIP-MOD-OC8*)
  * Multi IO Box - 2x (*HmIP-MIOB*)
  * Switch Circuit Board - 1x channels (*HmIP-PCBS*)
  * Switch Circuit Board - 2x channels (*HmIP-PCBS2*)
  * Printed Circuit Board Switch Battery (*HmIP-PCBS-BAT*)
  * Switch Actuator for heating systems – 2 channels (*HmIP-WHS2*)

* homematicip_cloud.weather
  * Weather Sensor – basic (*HmIP-SWO-B*)
  * Weather Sensor – plus (*HmIP-SWO-PL*)
  * Weather Sensor – pro (*HmIP-SWO-PR*)
  
## What to do, if a device is missing in Home Assistant

In order for a device to be integrated into Home Assistant, it must first be implemented in the upstream library. A dump of your configuration is required for this, which is then attached to a new issue in the [upstream lib's](https://github.com/coreGreenberet/homematicip-rest-api) GitHub repository.

1. Create a dump of your access point configuration in Home Assistant: 
  Developer Tools -> Services -> Select `homematicip_cloud.dump_hap_config` -> Execute. 
  The default dump is anonymized and is written to your configuration directory (`hmip_config_XXXX.json`).
2. Create a [new issue](https://github.com/coreGreenberet/homematicip-rest-api/issues/new) at this GitHub repository and attach the created dump file.

Please be patient, wait for the implementation and a new release of the upstream library.
Afterward, this device can be implemented into Home Assistant.
  
## Services

- `homematicip_cloud.activate_eco_mode_with_duration`: Activate eco mode with duration.
- `homematicip_cloud.activate_eco_mode_with_period`: Activate eco mode with period.
- `homematicip_cloud.activate_vacation`: Activates the vacation mode until the given time.
- `homematicip_cloud.deactivate_eco_mode`: Deactivates the eco mode immediately.
- `homematicip_cloud.deactivate_vacation`: Deactivates the vacation mode immediately.
- `homematicip_cloud.set_active_climate_profile`: Set the active climate profile index.
- `homematicip_cloud.dump_hap_config`: Dump the configuration of the Homematic IP Access Point(s).
- `homematicip_cloud.reset_energy_counter`: Reset energy counter of measuring actuators.

### Service Examples

`accesspoint_id` (SGTIN) is optional for all services and only relevant if you have multiple Homematic IP Accesspoints connected to HA. If empty, service will be called for all configured Homematic IP Access Points.
The `accesspoint_id` (SGTIN) can be found on top of the integration page, or on the back of your Homematic IP Accesspoint.

Activate eco mode with duration. 

```yaml
...
action:
  service: homematicip_cloud.activate_eco_mode_with_duration
  data:
    duration: 60
    accesspoint_id: 3014xxxxxxxxxxxxxxxxxxxx
```

Activate eco mode with period. 

```yaml
...
action:
  service: homematicip_cloud.activate_eco_mode_with_period
  data:
    endtime: 2019-09-17 18:00
    accesspoint_id: 3014xxxxxxxxxxxxxxxxxxxx
```

Activates the vacation mode until the given time.

```yaml
...
action:
  service: homematicip_cloud.activate_vacation
  data:
    endtime: 2019-09-17 18:00
    temperature: 18.5
    accesspoint_id: 3014xxxxxxxxxxxxxxxxxxxx
```

Deactivates the eco mode immediately.

```yaml
...
action:
  service: homematicip_cloud.deactivate_eco_mode
  data:
    accesspoint_id: 3014xxxxxxxxxxxxxxxxxxxx
```

Deactivates the vacation mode immediately.

```yaml
...
action:
  service: homematicip_cloud.deactivate_vacation
  data:
    accesspoint_id: 3014xxxxxxxxxxxxxxxxxxxx
```

Set the active climate profile index.

The index of the climate profile is 1-based. 
You can get the required index from the native Homematic IP App.

```yaml
...
action:
  service: homematicip_cloud.set_active_climate_profile
  data:
    entity_id: climate.livingroom
    climate_profile_index: 1
```

Dump the configuration of the Homematic IP Access Point(s).

```yaml
...
action:
  service: homematicip_cloud.dump_hap_config
  data:
    anonymize: True
```

Reset energy counter of measuring actuators.

```yaml
...
action:
  service: homematicip_cloud.reset_energy_counter
  data:
    entity_id: switch.livingroom
```


## Additional info

Push button devices are only available with a battery sensor. This is due to a limitation of the vendor API (eq3).
It's not possible to detect a key press event on these devices at the moment.

  * Remote Control - 8 buttons (*HmIP-RC8*)
  * Wall-mount Remote Control - 2-button (*HmIP-WRC2*)
  * Wall-mount Remote Control - 6-button (*HmIP-WRC6*)
  * Key Ring Remote Control - 4 buttons (*HmIP-KRC4*)
  * Key Ring Remote Control - alarm  (*HmIP-KRCA*)
