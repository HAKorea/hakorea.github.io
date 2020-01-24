---
title: Tado
description: Instructions on how to integrate Tado devices with Home Assistant.
logo: tado.png
ha_category:
  - Hub
  - Climate
  - Presence Detection
  - Sensor
ha_release: 0.41
ha_iot_class: Cloud Polling
ha_codeowners:
  - '@michaelarnauts'
---

The `tado` integration platform is used as an interface to the [my.tado.com](https://my.tado.com/) website.

There is currently support for the following device types within Home Assistant:

- Climate - for every tado zone.
- [Presence Detection](#presence-detection)
- Sensor - for some additional information of the zones.

## Configuration

To use your tado thermostats in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
tado:
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
```

{% configuration %}
username:
  description: Your username for [my.tado.com](https://my.tado.com/).
  required: true
  type: string
password:
  description: Your password for [my.tado.com](https://my.tado.com/).
  required: true
  type: string
fallback:
  description: Indicates if you want to fallback to Smart Schedule on the next Schedule change, or stay in Manual mode until you set the mode back to Auto.
  required: false
  type: boolean
  default: true
{% endconfiguration %}

The tado thermostats are internet connected thermostats. There exists an unofficial API at [my.tado.com](https://my.tado.com/), which is used by their website and now by this component.

It currently supports presenting the current temperature, the setting temperature and the current operation mode. Switching the mode is also supported. If no user is at home anymore, the devices are showing the away-state. Switching to away-mode is not supported.

## Presence Detection

The `tado` device tracker is using the [Tado Smart Thermostat](https://www.tado.com/) and its support for person presence detection based on smartphone location by geofencing.

This tracker uses the Tado API to determine if a mobile device is at home. It tracks all devices in your home that Tado knows about.

To use the Tado platform in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry for Tado
device_tracker:
  - platform: tado
    username: YOUR_USERNAME
    password: YOUR_PASSWORD
    home_id: YOUR_HOME_ID
```

{% configuration %}
username:
  description: The username for your Tado account.
  required: true
  type: string
password:
  description: The password for your Tado account.
  required: true
  type: string
home_id:
  description: The id of your home of which you want to track devices. If provided, the Tado device tracker will tack *all* devices known to Tado associated with this home. See below how to find it.
  required: false
  type: integer
{% endconfiguration %}

After configuration, your device has to be at home at least once before showing up as *home* or *away*.
Polling Tado API for presence information will happen at most once every 30 seconds.

See the [device tracker integration page](/integrations/device_tracker/) for instructions how to configure the people to be tracked. Beware that the Tado (v2) API does not provide GPS location of devices, only a bearing, therefore Home Assistant only uses `home`/`not-home` status.

### Finding your `home_id`

Find your `home_id` by browsing to `https://my.tado.com/api/v2/me?username=YOUR_USERNAME&password=YOUR_PASSWORD`. There you'll see something like the following:

```json
{
  "name": "Mark",
  "email": "your@email.tld",
  "username": "your@email.tld",
  "homes": [
    {
      "id": 12345,
      "name": "Home Sweet Home"
    }
  ],
  "locale": "en_US",
  "mobileDevices": []
}
```

In this example `12345` is the `home_id` you'll need to configure.
