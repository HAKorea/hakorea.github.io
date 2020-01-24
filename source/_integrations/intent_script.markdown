---
title: Intent Script
description: Instructions on how to setup scripts to run on intents.
logo: home-assistant.png
ha_category:
  - Intent
ha_release: '0.50'
ha_quality_scale: internal
---

The `intent_script` integration allows users to configure actions and responses to intents. Intents can be fired by any integration that supports it. Examples are [Alexa](/integrations/alexa/) (Amazon Echo), [Dialogflow](/integrations/dialogflow/) (Google Assistant) and [Snips](/integrations/snips/).

```yaml
# Example configuration.yaml entry
intent_script:
  GetTemperature:  # Intent type
    speech:
      text: We have {% raw %}{{ states.sensor.temperature }}{% endraw %} degrees
    action:
      service: notify.notify
      data_template:
        message: Hello from an intent!
```

Inside an intent we can define these variables:

{% configuration %}
intent:
  description: Name of the intent. Multiple entries are possible.
  required: true
  type: map
  keys:
    action:
      description: Defines an action to run to intents.
      required: false
      type: action
    async_action:
      description: Set to True to have Home Assistant not wait for the script to finish before returning the intent response.
      required: false
      default: false
      type: boolean
    card:
      description: Card to display.
      required: false
      type: map
      keys:
        type:
          description: Type of card to display.
          required: false
          default: simple
          type: string
        title:
          description: Title of the card to display.
          required: true
          type: template
        content:
          description: Contents of the card to display.
          required: true
          type: template
    speech:
      description: Text or template to return.
      required: false
      type: map
      keys:
        type:
          description: Type of speech.
          required: false
          default: plain
          type: string
        text:
          description: Text to speech.
          required: true
          type: template
{% endconfiguration %}
