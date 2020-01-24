---
title: Yandex TTS
description: Instructions on how to setup Yandex SpeechKit TTS with Home Assistant.
logo: yandex.png
ha_category:
  - Text-to-speech
ha_release: 0.36
---

The `yandextts` text-to-speech platform uses [Yandex SpeechKit](https://tech.yandex.com/speechkit/) Text-to-Speech engine to read a text with natural sounding voices.

## Configuration

To enable text-to-speech with Yandex SpeechKit, add the following lines to your `configuration.yaml`:

```yaml
# Example configuration.yaml entry
tts:
  - platform: yandextts
    api_key: THE_API_KEY
```

{% configuration %}
api_key:
  description: The API Key for use this service.
  required: true
  type: string
language:
  description: "The language to use. Supported languages are `en-US`, `ru-RU`, `uk-UK` and `tr-TR`."
  required: false
  type: string
  default: "`en-US`"
codec:
  description: "The audio codec. Supported codecs are `mp3`, `wav` and `opus`."
  required: false
  type: string
  default: "`mp3`"
voice:
  description: "The speaker voice. Supported female voices are `jane`, `oksana`, `alyss`, `omazh`, `silaerkan`, `nastya`, `sasha`, `tanya`, `tatyana_abramova`, `voicesearch`, and `zombie`. Male voices are `zahar`, `ermil`, `levitan`, `ermilov`, `kolya`, `kostya`, `nick`, `erkanyavas`, `zhenya`, `anton_samokhvalov`, `ermil_with_tuning`, `robot`, `dude`, and `smoky`."
  required: false
  type: string
  default: "`zahar`"
emotion:
  description: "The speaker emotional intonation. Supported emotions are `good` (friendly), `evil` (angry) and `neutral`"
  required: false
  type: string
  default: "`neutral`"
speed:
  description: The speech speed. Highest speed is `3` and lowest `0,1`
  required: false
  type: float
  default: "`1`"
{% endconfiguration %}

Please check the [API documentation](https://tech.yandex.com/speechkit/cloud/doc/guide/concepts/tts-http-request-docpage/) for details. It seems that the English version of documentation is outdated. You could request an API key [by email](https://tech.yandex.com/speechkit/cloud/) or [online](https://developer.tech.yandex.ru/).

## Full configuration example

The configuration sample below shows how an entry can look like:

```yaml
# Example configuration.yaml entry
tts:
  - platform: yandextts
    api_key: YOUR_API_KEY
    language: 'ru-RU'
    codec: mp3
    voice: oksana
    emotion: evil
    speed: 2
```
