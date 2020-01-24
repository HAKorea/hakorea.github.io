---
title: EPH Controls
description: Instructions on how to integrate EPH Controls EMBER thermostats within Home Assistant.
logo: ephcontrolsember.png
ha_category:
  - Climate
ha_release: 0.57
ha_iot_class: Local Polling
ha_codeowners:
  - '@ttroy50'
---

The `ephember` climate platform lets you control [EPH Controls](https://emberapp.ephcontrols.com/) thermostats. The module only works if you have a WiFi gateway to control your EPH system and an account on the EMBER app.

To set it up, add the following information to your `configuration.yaml` file:

```yaml
climate:
  - platform: ephember
    username: YOUR_EMAIL
    password: YOUR_PASSWORD
```

A single interface can handle up to 32 connected devices.

{% configuration %}
username:
  description: The email address you used to sign up to the EMBER app.
  required: true
  type: string
password:
  description: The password you used to sign up to the EMBER app.
  required: true
  type: string
{% endconfiguration %}

The supported operation modes map to the ON/OFF period selection of your timeswitch / EMBER app. These include:

- **Auto** The timeswitch operates 3 on / off periods per day.
- **On** The timeswitch is permanently on.
- **Off** The timeswitch is permanently off.

If **All Day** is selected in the EMBER app it will show as **Auto** in Home Assistant.
