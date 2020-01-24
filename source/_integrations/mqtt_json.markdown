---
title: MQTT JSON
description: Instructions on how to use JSON MQTT to track devices in Home Assistant.
logo: mqtt.png
ha_category:
  - Presence Detection
ha_iot_class: Configurable
ha_release: 0.44
---

The `mqtt_json` device tracker platform allows you to detect presence by monitoring an MQTT topic for new locations. To use this platform, you specify a unique topic for each device.

## Configuration

To use this device tracker in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
device_tracker:
  - platform: mqtt_json
    devices:
      paulus_oneplus: location/paulus
      annetherese_n4: location/annetherese
```

{% configuration %}
devices:
  description: List of devices with their topic.
  required: true
  type: list
qos:
  description: The QoS level of the topic.
  required: false
  type: string
{% endconfiguration %}

## Usage

This platform receives JSON formatted payloads containing GPS information, for example:

```json
{"longitude": 1.0,"gps_accuracy": 60,"latitude": 2.0,"battery_level": 99.9}
```

Where `longitude` is the longitude, `latitude` is the latitude, `gps_accuracy` is the accuracy in meters, `battery_level` is the current battery level of the device sending the update.
`longitude` and `latitude` are required keys, `gps_accuracy` and `battery_level` are optional.
