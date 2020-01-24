---
title: Home Asssitant WebSocket API
description: Instructions on how to setup the WebSocket API within Home Assistant.
logo: home-assistant.png
ha_category:
  - Other
ha_release: 0.34
ha_quality_scale: internal
ha_codeowners:
  - '@home-assistant/core'
---

The `websocket_api` integration set up a WebSocket API and allows one to interact with a Home Assistant instance that is running headless. This integration depends on the [`http` component](/integrations/http/).

<div class='note warning'>

It is HIGHLY recommended that you set the `api_password`, especially if you are planning to expose your installation to the internet.

</div>

## Configuration

```yaml
# Example configuration.yaml entry
websocket_api:
```

For details to use the WebSocket API, please refer to the [WebSocket API documentation](/developers/websocket_api/) .

## Track current connections

The websocket API provides a sensor that will keep track of the number of current connected clients. You can add it by adding the following to your configuration:

```yaml
# Example configuration.yaml entry
sensor:
  platform: websocket_api
```
