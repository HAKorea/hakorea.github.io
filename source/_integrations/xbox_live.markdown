---
title: Xbox Live
description: Instructions on how to set up Xbox Live sensors in Home Assistant.
logo: xbox-live.png
ha_category:
  - Social
ha_iot_class: Cloud Polling
ha_release: 0.28
ha_codeowners:
  - '@MartinHjelmare'
---

The Xbox Live integration is able to track [Xbox](https://xbox.com/) profiles.

To use this sensor you need a free API key from
[XboxAPI.com](https://xboxapi.com/).
Please also make sure to connect your Xbox account on that site.

The configuration requires you to specify XUIDs which are the unique identifiers
for profiles. These can be determined on [XboxAPI.com](https://xboxapi.com/) by
either looking at your own profile page or using their interactive documentation
to search for gamertags. Sensor names default to the gamertag associated with an XUID.

To use the Xbox Live sensor in your installation,
add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: xbox_live
    api_key: YOUR_API_KEY
    xuid:
      - account1
      - account2
```

{% configuration %}
api_key:
  description: Your API key from [XboxAPI.com](https://xboxapi.com/).
  required: true
  type: string
xuid:
  description: Array of profile XUIDs to be tracked.
  required: true
  type: list
{% endconfiguration %}
