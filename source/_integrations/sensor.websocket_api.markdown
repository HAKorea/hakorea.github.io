---
title: "Websocket Connections Sensor"
description: "Instructions on how to count connected clients within Home Assistant."
logo: home-assistant.png
ha_category:
  - Utility
ha_release: 0.33
ha_iot_class: Local Push
ha_quality_scale: internal
---

The `websocket_api` sensor platform shows how many clients are connected to the stream API.

## Configuration

To add the connected clients to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: websocket_api
```

### Note

This replaces the previous `api_streams` sensor.
