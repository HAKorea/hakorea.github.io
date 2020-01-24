---
title: Atome Linky
description: Integrate Atome Linky consumption data within Home Assistant.
logo: total_direct_energie.png
ha_release: 0.99
ha_category:
  - Energy
  - Sensor
ha_iot_class: Cloud Polling
ha_codeowners:
  - '@baqs'
---

The `atome` sensor platform is retrieving the consumption of your home from the [Direct Energy Atome electric meter](https://total.direct-energie.com/particuliers/electricite/compteur-linky/atome).
This special little device is connected to a Linky Electric Meter, and sends live data to a cloud platform.

As there is no official documentation for the API, the component retrieves data from the API used in the Atome mobile app, [hosted here](https://esoftlink.esoftthings.com/).

## Configuration

To use it, you need to order the device directly from "Total Direct Energie" Mobile App. Then you need to follow up the installation (covered in the Atome App).
The configuration (see below) needs your Atome username & password you created during the initialization of the Atome device.

Next, add the Atome sensor to your `configuration.yaml` file like below:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: atome
    username: YOUR_ATOME_USERNAME
    password: YOUR_ATOME_PASSWORD
```

{% configuration %}
username:
  description: The Atome account username.
  required: true
  type: string
password:
  description: The Atome account password.
  required: true
  type: string
{% endconfiguration %}
