---
title: Meraki
description: Instructions on how to integrate Meraki-based presence detection into Home Assistant.
logo: meraki.png
ha_category:
  - Presence Detection
ha_release: '0.60'
---

Use your Meraki AP as device tracker. Note that Meraki will see all devices, not only connected to the network.

### Prerequisites

1. Go to Network-wide/General page, and find the Location and scanning section.
1. Make sure analytics and Scanning API are both enabled.
1. Make note of the Validator string, which will be used in the `device_tracker` configuration.
1. Click **Add a Post URL**:
  1. Set the Post URL to `https://YOUR_HOSTNAME/api/meraki?api_password=YOUR_HASS_PASSWORD`
  1. Set the Secret to a randomly generated string, and make note of it for the `device_tracker` configuration.
  1. Make sure the API Version is set to `2.0`.
  1. Hit **Save** in the bottom right of the page.

## Configuration

After you configure access to the Meraki CMX API, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
device_tracker:
  - platform: meraki
    secret: your_secret
    validator: meraki_validator
```

{% configuration %}
  secret:
    description: Secret code added in Meraki.
    required: true
    type: string
  validator:
    description: Validation string from Meraki.
    required: true
    type: string
{% endconfiguration %}
