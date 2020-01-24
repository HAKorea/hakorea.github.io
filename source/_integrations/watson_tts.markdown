---
title: IBM Watson TTS
description: Instructions on how to setup IBM Watson TTS with Home Assistant.
logo: watson_tts.png
ha_category:
  - Text-to-speech
ha_release: 0.94
ha_codeowners:
  - '@rutkai'
---

The `watson_tts` text-to-speech platform that works with [IBM Watson Cloud](https://www.ibm.com/watson/services/text-to-speech/) to create the spoken output.
Watson is a paid service via IBM Cloud but there is a decent [free tier](https://www.ibm.com/cloud/watson-text-to-speech/pricing) which offers 10000 free characters every month.

## Setup

For supported formats and voices please go to [IBM Cloud About section](https://cloud.ibm.com/docs/services/text-to-speech?topic=text-to-speech-about#about).

To get started please read the [Getting started tutorial](https://cloud.ibm.com/docs/services/text-to-speech?topic=text-to-speech-gettingStarted#gettingStarted).

## Configuration

To configure Watson TTS, add the following lines to your `configuration.yaml`:

```yaml
# Example configuration.yaml entry
tts:
  - platform: watson_tts
    watson_apikey: YOUR_GENERATED_APIKEY
```

You can get these tokens after you generated the credentials on the IBM Cloud console:

<p class='img'>
  <img src='{{site_root}}/images/screenshots/watson_tts_screen.png' />
</p>

{% configuration %}
watson_url:
  description: "The endpoint to which the service will connect."
  required: false
  type: string
  default: https://stream.watsonplatform.net/text-to-speech/api
watson_apikey:
  description: "Your secret apikey generated on the IBM Cloud admin console."
  required: true
  type: string
voice:
  description: Voice name to be used.
  required: false
  type: string
  default: en-US_AllisonVoice
output_format:
  description: "Override the default output format. Supported formats: `audio/flac`, `audio/mp3`, `audio/mpeg`, `audio/ogg`, `audio/ogg;codecs=opus`, `audio/ogg;codecs=vorbis`, `audio/wav`"
  required: false
  type: string
  default: audio/mp3
{% endconfiguration %}

## Usage

Say to all `media_player` device entities:

```yaml
- service: tts.watson_tts_say
  data_template:
    message: 'Hello from Watson'
```

or

```yaml
- service: tts.watson_tts_say
  data_template:
    message: >
      <speak>
          Hello from Watson
      </speak>
```

Say to the `media_player.living_room` device entity:

```yaml
- service: tts.watson_tts_say
  data_template:
    entity_id: media_player.living_room
    message: >
      <speak>
          Hello from Watson
      </speak>
```

Say with break:

```yaml
- service: tts.watson_tts_say
  data_template:
    message: >
      <speak>
          Hello from
          <break time=".9s" />
          Watson
      </speak>
```
