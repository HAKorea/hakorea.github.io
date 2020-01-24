---
title: Baidu
description: Instructions on how to setup Baidu TTS with Home Assistant.
logo: baiducloud.png
ha_category:
  - Text-to-speech
ha_release: 0.59
---

The `baidu` text-to-speech platform uses [Baidu TTS engine](https://cloud.baidu.com/product/speech/tts) to read a text with natural sounding voices.

## Configuration

To get started, add the following lines to your `configuration.yaml`:

```yaml
#Example configuration.yaml entry
tts:
  - platform: baidu
    app_id: YOUR_APPID
    api_key: YOUR_APIKEY
    secret_key: YOUR_SECRETKEY
```

{% configuration %}
app_id:
  description: The App ID for the use this service, must be already registered on Baidu.
  required: true
  type: string
api_key:
  description: The API key from Baidu.
  required: true
  type: string
secret_key:
  description: The secret key from Baidu.
  required: true
  type: string
speed:
  description: Audio speed from 0 to 9.
  required: false
  type: integer
  default: 5
pitch:
  description: Audio pitch from 0 to 9.
  required: false
  type: integer
  default: 5
volume:
  description: Audio volume from 0 to 15.
  required: false
  type: integer
  default: 5
person:
  description: The voice type. You choose one from 0, 1, 3 or 4.
  required: false
  type: integer
  default: 0
{% endconfiguration %}

At the moment, `zh` is the only supported language and therefore the default value.
