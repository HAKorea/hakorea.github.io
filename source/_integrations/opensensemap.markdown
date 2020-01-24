---
title: openSenseMap
description: Instructions on how to setup openSenseMap sensors in Home Assistant.
logo: opensensemap.png
ha_category:
  - Health
ha_release: 0.85
ha_iot_class: Cloud Polling
---

The `opensensemap` air quality platform will query the open data API of [openSenseMap.org](https://opensensemap.org/) to monitor air quality sensor station.

## Setup

To get the ID of a station you need to select it on the [openSense map](https://opensensemap.org/) and find it in the addressbar of your browser. It's the last part of the URL, e.g., `5b450e565dc1ec001bf7cd1d` [https://opensensemap.org/explore/5b450e565dc1ec001bf7cd1d](https://opensensemap.org/explore/5b450e565dc1ec001bf7cd1d).

## Manual Configuration

To enable this platform, add the following lines to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
air_quality:
  - platform: opensensemap
    station_id: STATION_ID
```

{% configuration %}
station_id:
  description: The ID of the station to monitor.
  required: true
  type: string
name:
  description: Name of the sensor to use in the frontend.
  required: false
  default: Station name
  type: string
{% endconfiguration %}
