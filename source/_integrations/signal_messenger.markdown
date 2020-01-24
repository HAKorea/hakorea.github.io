---
title: Signal Messenger
description: Instructions on how to integrate Signal Messenger within Home Assistant.
logo: signal_messenger.png
ha_category:
  - Notifications
ha_release: 0.104
ha_codeowners:
  - '@bbernhard'
---

The `signal_messenger` integration uses the [Signal Messenger REST API](https://github.com/bbernhard/signal-cli-rest-api) to deliver notifications from Home Assistant to your Android or iOS device.

## Setup
 
The requirements are:

- You need to set up the Signal Messenger REST API. 
- You need a spare phone number to register with the Signal Messenger service. 


Please follow those [instructions](https://github.com/bbernhard/signal-cli-rest-api/blob/master/doc/HOMEASSISTANT.md), to set up the Signal Messenger REST API. 


## Configuration

To send Signal Messenger notifications with Home Assistant, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry for Signal Messenger 
notify:
  - name: signal
    platform: signal_messenger
    url: "http://127.0.0.1:8080" # the URL where the Signal Messenger REST API is listening 
    number: "YOUR_PHONE_NUMBER" # the sender number
    recipients: # one or more recipients
      - "RECIPIENT1"
```

{% configuration %}
name:
  description: Setting the optional parameter `name` allows multiple notifiers to be created. The notifier will bind to the service `notify.NOTIFIER_NAME`.
  required: false
  type: string
  default: notify
url:
  description: The URL where the Signal Messenger REST API listens for incoming requests. 
  required: true
  type: string
number:
  description: The sender number.
  required: true
  type: string
recipients:
  description: A list of recipients.
  required: true
  type: string
{% endconfiguration %}

## Examples

A few examples on how to use this integration.

### Text message

```yaml
...
action:
  service: notify.NOTIFIER_NAME
  data:
    message: "That's an example that sends a simple text message to the recipients specified in the configuration.yaml"
```

### Text message with an attachment

```yaml
...
action:
  service: notify.NOTIFIER_NAME
  data:
    message: "Alarm in the living room!"
    data:
      attachment: "/tmp/surveillance_camera.jpg"
```
