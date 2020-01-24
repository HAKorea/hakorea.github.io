---
title: Slide
description: Instructions on how to integrate the Innovation in Motion Slide covers with Home Assistant.
logo: slide.png
ha_category:
  - Hub
  - Cover
ha_iot_class: Cloud Polling
ha_release: 0.99
ha_codeowners:
  - '@ualex73'
---

The `slide` implementation allows you to integrate your [slide.store](https://slide.store/) devices in Home Assistant using the [official API](https://documenter.getpostman.com/view/6223391/S1Lu2pSf?version=latest).

### Configuration

```yaml
# Example configuration.yaml entry
slide:
  username: YOUR_SLIDE_APP_USERNAME
  password: YOUR_SLIDE_APP_PASSWORD
```

{% configuration %}
username:
  description: Username needed to log in to Slide App.
  required: true
  type: string
password:
  description: Password needed to log in to Slide App.
  required: true
  type: string
scan_interval:
  description: "Minimum time interval between updates."
  required: false
  default: 30 seconds
  type: integer
{% endconfiguration %}
