---
title: Lutron Caseta
description: Instructions on how to use Lutron Caseta devices with Home Assistant.
logo: lutron.png
ha_category:
  - Hub
  - Cover
  - Light
  - Scene
  - Switch
  - Fan
ha_release: 0.41
ha_iot_class: Local Polling
---

[Lutron](http://www.lutron.com/) is an American lighting control company. They have several lines of home automation devices that manage light switches, dimmers, occupancy sensors, HVAC controls, etc. The `lutron_caseta` integration in Home Assistant is responsible for communicating with the Lutron Caseta Smart Bridge for the [Caseta](https://www.casetawireless.com/) product line of dimmers, switches and shades.

This integration only supports the [Caseta](https://www.casetawireless.com/) line of products. Both Smart Bridge (L-BDG2-WH) and Smart Bridge PRO (L-BDGPRO2-WH) models are supported. For the RadioRA 2 product line, see the [Lutron component](/integrations/lutron/).

The currently supported Caseta devices are:

- Wall and plug-in dimmers as [lights](#light)
- Wall switches as [switches](#switch)
- Scenes as [scenes](#scene)
- Lutron shades as [covers](#cover)
- Lutron smart [fan](#fan) speed control

When configured, the `lutron_caseta` integration will automatically discover the currently supported devices as setup in the Lutron Smart Bridge. The name assigned in the Lutron mobile app will be used to form the `entity_id` used in Home Assistant. e.g., a dimmer called 'Lamp' in a room called 'Bedroom' becomes `light.bedroom_lamp` in Home Assistant.

To use Lutron Caseta devices in your installation, you must first log in to your Lutron account and generate a certificate that allows Home Assistant to connect to your bridge. This can be accomplished by downloading and executing [get_lutron_cert.py](https://github.com/gurumitts/pylutron-caseta/blob/master/get_lutron_cert.py), which will generate three files: caseta.key, caseta.crt, caseta-bridge.crt when you run it. See the instructions at the top of the script for more information.

Once you have the three necessary files, place them in your configuration directory and add the following to your `configuration.yaml`:

```yaml
# Example configuration.yaml entry
lutron_caseta:
    host: IP_ADDRESS
    keyfile: caseta.key
    certfile: caseta.crt
    ca_certs: caseta-bridge.crt
```

{% configuration %}
  host:
    required: true
    description: The IP address of the Lutron Smart Bridge.
    type: string
  keyfile:
    required: true
    description: The private key that Home Assistant will use to authenticate to the bridge.
    type: string
  certfile:
    required: true
    description: The certificate chain that Home Assistant will use to authenticate to the bridge.
    type: string
  ca_certs:
    required: true
    description: The list of certificate authorities (usually only one) that Home Assistant will expect when connecting to the bridge.
    type: string
{% endconfiguration %}

<div class='note'>

It is recommended to assign a static IP address to your Lutron Smart Bridge. This ensures that it won't change IP address, so you won't have to change the `host` if it reboots and comes up with a different IP address.
<br>
Use a DHCP reservation on your router to reserve the address or in the PRO model of the Smart Bridge, set the IP address under Network Settings in the Advanced / Integration menu in the mobile app.

</div>

To get Lutron Caseta roller, honeycomb shades, lights, scene and switch working with Home Assistant. First follow the instructions for the general Lutron Caseta integration above.

## Cover

After setup, shades will appear in Home Assistant using an `entity_id` based on the name used in the Lutron mobile app. For example, a shade called 'Living Room Window' will appear in Home Assistant as `cover.living_room_window`.

For more information on working with shades in Home Assistant, see the [Covers component](/integrations/cover/).

Available services: `cover.open_cover`, `cover.close_cover` and `cover.set_cover_position`. Cover `position` ranges from `0` for fully closed to `100` for fully open.

## Light

After setup, dimmable lights including wall and plug-in dimmers will appear in Home Assistant using an `entity_id` based on the name used in the Lutron mobile app. For example, a light called 'Bedroom Lamp' will appear in Home Assistant as `light.bedroom_lamp`.

For non-dimmable lights or switched loads, see the switch section on this page.

For more information on working with lights in Home Assistant, see the [Lights component](/integrations/light/).

Available services: `light.turn_on`, `light.turn_off` and `light.toggle`. The `light.turn_on` service supports attributes `brightness` and `brightness_pct`.

## Scene

The Lutron Caseta scene platform allows you to control your Smart Bridge Scenes that are created in the Lutron mobile app.

After setup, scenes will appear in Home Assistant using an `entity_id` based on the name used in the Lutron mobile app. For example, a scene called 'Entertain' will appear in Home Assistant as `scene.entertain`.

For more information on working with scenes in Home Assistant, see the [Scenes component](/integrations/scene/).

Available services: `scene.turn_on`.

## Switch

After setup, switches will appear in Home Assistant using an `entity_id` based on the name used in the Lutron mobile app. For example, a light switch called 'Master Bathroom Vanity' will appear in Home Assistant as `switch.master_bathroom_vanity`.

For dimmable lights including wall and plug-in dimmers, see the light section on this page.

For more information on working with switches in Home Assistant, see the [Switches component](/integrations/switch/).

Available services: `switch.turn_on` and `switch.turn_off`.

## Fan

After setup, fans will appear in Home Assistant using an `entity_id` based on the name used in the Lutron mobile app. For example, a light switch called 'Master Bathroom Vanity' will appear in Home Assistant as `fan.master_bedroom_ceiling_fan`.

For more information on working with fans in Home Assistant, see the [Fans component](/components/fan/).

Available services: `fan.turn_on`, `fan.turn_off`, and `fan.set_speed`.
