---
title: FFmpeg Noise
description: Instructions on how to integrate an FFmpeg-based noise binary sensor
logo: ffmpeg.png
ha_category:
  - Image Processing
ha_release: 0.27
---

The `ffmpeg` platform allows you to use any video or audio feed with [FFmpeg](https://www.ffmpeg.org/) for various sensors in Home Assistant.

<div class='note'>

If the `ffmpeg` process is broken, the sensor will be unavailable. To control the ffmpeg process of sensor, use the service *ffmpeg.start*, *ffmpeg.stop*, *ffmpeg.restart*.

</div>

## Configuration

To add FFmpeg with noise detection to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
binary_sensor:
  - platform: ffmpeg_noise
    input: FFMPEG_SUPPORTED_INPUT
```

{% configuration %}
input:
  description: An FFmpeg-compatible input file, stream, or feed.
  required: true
  type: string
name:
  description: Override the name of your camera.
  required: false
  type: string
initial_state:
  description: Start ffmpeg with home-assistant.
  required: false
  type: boolean
  default: true
peak:
  description: The threshold of detecting noise, in dB. 0 is very loud and -100 is low.
  required: false
  type: integer
  default: -30
duration:
  description: How long the noise needs to be over the peak to trigger the state.
  required: false
  type: integer
  default: 1
reset:
  description: The time to reset the state after no new noise is over the peak.
  required: false
  type: integer
  default: 20
extra_arguments:
  description: Extra options to pass to `ffmpeg`, like audio frequency filtering.
  required: false
  type: string
output:
  description: Allows you to send the audio output of this sensor to an Icecast server or other FFmpeg-supported output, e.g., to stream with Sonos after a state is triggered.
  required: false
  type: string
{% endconfiguration %}

To experiment with values:

```bash
$ ffmpeg -i YOUR_INPUT -vn -filter:a silencedetect=n=-30dB:d=1 -f null -
```
