---
title: Light
description: Instructions on how to setup your lights with Home Assistant.
logo: home-assistant.png
ha_category:
  - Light
ha_release: pre 0.7
ha_quality_scale: internal
---

This integration allows you to track and control various light bulbs. Read the integration documentation for your particular light hardware to learn how to enable it.

### Default turn-on values

To set the default color and brightness values when the light is turned on, create a custom `light_profiles.csv` (as described below in the `profile` attribute of `light.turn_on`).

The `.default` suffix should be added to the entity identifier of each light to define a default value, e.g., for `light.ceiling_2` the `id` field is `light.ceiling_2.default`. To define a default for all lights, the identifier `group.all_lights.default` can be used. Individual settings always supercede the `all_lights` default setting.

### Service `light.turn_on`

Turns one light on or multiple lights on using [groups]({{site_root}}/integrations/group/).

Most lights do not support all attributes. You can check the integration documentation of your particular light for hints, but in general, you will have to try things out and see what works.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `entity_id` | no | String or list of strings that point at `entity_id`s of lights. To target all the lights use all as `entity_id`.
| `transition` | yes | Number that represents the time (in seconds) the light should take to transition to the new state.
| `profile` | yes | String with the name of one of the [built-in profiles](https://github.com/home-assistant/home-assistant/blob/master/homeassistant/components/light/light_profiles.csv) (relax, energize, concentrate, reading) or one of the custom profiles defined in `light_profiles.csv` in the current working directory.  Light profiles define an xy color and a brightness. If a profile is given and a brightness then the profile brightness will be overwritten.
| `hs_color` | yes | A list containing two floats representing the hue and saturation of the color you want the light to be. Hue is scaled 0-360, and saturation is scaled 0-100.
| `xy_color` | yes | A list containing two floats representing the xy color you want the light to be. Two comma-separated floats that represent the color in XY. You can find a great chart here: [Hue Color Chart](https://developers.meethue.com/documentation/core-concepts#color_gets_more_complicated).
| `rgb_color` | yes | A list containing three integers between 0 and 255 representing the RGB color you want the light to be. Three comma-separated integers that represent the color in RGB, within square brackets. Note that the specified RGB value will not change the light brightness, only the color.
| `white_value` | yes | Integer between 0 and 255 for how bright a dedicated white LED should be.
| `color_temp` | yes | An integer in mireds representing the color temperature you want the light to be.
| `kelvin` | yes | Alternatively, you can specify the color temperature in Kelvin.
| `color_name` | yes | A human-readable string of a color name, such as `blue` or `goldenrod`. All [CSS3 color names](https://www.w3.org/TR/css-color-3/#svg-color) are supported.
| `brightness` | yes | Integer between 0 and 255 for how bright the light should be, where 0 means the light is off, 1 is the minimum brightness and 255 is the maximum brightness supported by the light.
| `brightness_pct`| yes | Alternatively, you can specify brightness in percent (a number between 0 and 100), where 0 means the light is off, 1 is the minimum brightness and 100 is the maximum brightness supported by the light.
| `flash` | yes | Tell light to flash, can be either value `short` or `long`.
| `effect`| yes | Applies an effect such as `colorloop` or `random`.

<div class='note'>

In order to apply attributes to an entity, you will need to add `data:` to the configuration. See example below

</div>

```yaml
# Example configuration.yaml entry
automation:
- id: one
  alias: Turn on light when motion is detected
  trigger:
    - platform: state
      entity_id: binary_sensor.motion_1
      to: 'on'
  action:
    - service: light.turn_on
      data:
        entity_id: light.living_room
        brightness: 255
        kelvin: 2700
```
```yaml
# Ledlist morning on, red
- id: llmor
  alias: Stair morning on
  trigger:
  - at: '05:00'
    platform: time
  action:
    - service: light.turn_on
      data:
        entity_id: light.ledliststair
        brightness: 130
        rgb_color: [255,0,0]
```

### Service `light.turn_off`

Turns one or multiple lights off.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `entity_id` | no | String or list of strings that point at `entity_id`s of lights. To target all the lights use all as `entity_id`.
| `transition` | yes | Integer that represents the time the light should take to transition to the new state in seconds.

### Service `light.toggle`

Toggles the state of one or multiple lights using [groups]({{site_root}}/integrations/group/). 
Takes the same arguments as [`turn_on`](#service-lightturn_on) service.

*Note*: If `light.toggle` is used for a group of lights, it will toggle the individual state of each light.
