---
title: Garadget
description: Instructions on how to integrate Garadget covers within Home Assistant.
logo: garadget.png
ha_category:
  - Cover
ha_release: 0.32
ha_iot_class: Cloud Polling
---

The `garadget` cover platform lets you control [Garadget](https://www.garadget.com/) garage door futurizers through Home Assistant.

## Configuration

To enable Garadget Covers in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry

 cover:
  - platform: garadget
    covers:
      first_garage:
        device: 190028001947343412342341
        username: YOUR_USERNAME
        password: YOUR_PASSWORD
        name: first_garage
      second_garage:
        device: 4c003f001151353432134214
        access_token: df4cc785ff818f2b01396c44142342fccdef
        name: second_garage

```

{% configuration %}
covers:
  description: List of your doors.
  required: true
  type: list
  covers:
      keys:
        device:
          description: This is the device id from your Garadget portal.
          required: true
          type: string
        username:
          description: Your Garadget account username.
          required: true
          type: string
        password:
          description: Your Garadget account password.
          required: true
          type: string
        access_token:
          description: A generated `access_token` from your Garadget account.
          required: true
          type: string
        name:
          description: me to use in the frontend, will use name configured in Garadget otherwise.
          required: false
          default: Garadget
          type: string
{% endconfiguration %}

If provided, the **access_token** will be used, otherwise the **username** and **password** will be used to automatically generate an access token at start time.

## Example

<p class='img'>
  <img src='{{site_root}}/images/integrations/garadget/cover_garadget_details.png' />
</p>

{% raw %}
```yaml
# Related configuration.yaml entry
cover:
  - platform: garadget
    covers:
      garadget:
        device: 190028001947343412342341
        access_token: !secret garadget_access_token
        name: Garage door

sensor:
  - platform: template
    sensors:
      garage_door_status:
        friendly_name: 'State of the door'
        value_template: "{{ states('cover.garage_door') }}"
      garage_door_time_in_state:
        friendly_name: 'Since'
        value_template: "{{ state_attr('cover.garage_door', 'time_in_state') }}"
      garage_door_wifi_signal_strength:
        friendly_name: 'WiFi strength'
        value_template: "{{ state_attr('cover.garage_door', 'wifi_signal_strength') }}"
        unit_of_measurement: 'dB'

group:
  garage_door:
    name: Garage door
    entities:
      - cover.garage_door
      - sensor.garage_door_status
      - sensor.garage_door_time_in_state
      - sensor.garage_door_wifi_signal_strength

customize:
  sensor.garage_door_time_in_state:
    icon: mdi:timer-sand
  sensor.garage_door_wifi_signal_strength:
    icon: mdi:wifi
```
{% endraw %}

Some of the Garadget sensors can create a lot of clutter in the logbook.  Use this section of code in your `configuration.yaml` to exclude those entries.

```yaml
logbook:
  exclude:
    entities:
      - sensor.garage_door_time_in_state
      - sensor.garage_door_wifi_signal_strength
```

As of firmware release 1.17 the garadget device has native support for MQTT. The options allow the end-user to configure the device in the following ways 'cloud only', 'cloud and MQTT' or 'MQTT only'.

For configuration of the garadget as a MQTT cover:

```yaml
cover:
  - platform: mqtt
    name: "Garage Door"
    command_topic: "garadget/device_name/command"
    state_topic: "garadget/device_name/status"
    payload_open: "open"
    payload_close: "close"
```
